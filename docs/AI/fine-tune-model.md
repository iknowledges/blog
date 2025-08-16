# 微调预训练模型

- yelp_review_full: https://hf-mirror.com/datasets/yelp_review_full
- bert-base-cased: https://hf-mirror.com/bert-base-cased
- accuracy: https://hf-mirror.com/spaces/evaluate-metric/accuracy

## 使用PyTorch Trainer进行训练

```python
from datasets import load_dataset
from transformers import AutoTokenizer
from transformers import AutoModelForSequenceClassification
import numpy as np
import evaluate
from transformers import TrainingArguments, Trainer


# 加载Yelp评论数据集，cache_dir指定本地缓存路径
dataset = load_dataset("E:\\huggingface\\datasets_modules\\yelp_review_full", cache_dir="E:\\huggingface\\datasets")
print(dataset["train"][100])

# 加载tokenizer来处理文本
tokenizer = AutoTokenizer.from_pretrained("E:\\huggingface\\bert-base-cased", model_max_length=322)

def tokenize_function(examples):
    # 填充和截断操作以处理可变的序列长度
    return tokenizer(examples["text"], padding="max_length", truncation=True)

tokenized_datasets = dataset.map(tokenize_function, batched=True)

# 从完整数据集提取一个较小子集来进行微调，以减少训练的时间
small_train_dataset = tokenized_datasets["train"].shuffle(seed=42).select(range(1000))
small_eval_dataset = tokenized_datasets["test"].shuffle(seed=42).select(range(1000))

# 加载模型，num_labels指定期望的标签数量
model = AutoModelForSequenceClassification.from_pretrained("E:\\huggingface\\bert-base-cased", num_labels=5)

# 加载Evaluate库的accuracy函数来评估模型性能
metric = evaluate.load("E:\\huggingface\\evaluate\\accuracy")

def compute_metrics(eval_pred):
    logits, labels = eval_pred
    predictions = np.argmax(logits, axis=-1)
    return metric.compute(predictions=predictions, references=labels)

# 创建TrainingArguments类，其中包含所有超参数以及用于激活不同训练选项的标志
# 指定evaluation_strategy参数，以在每个epoch结束时展示评估指标
training_args = TrainingArguments(output_dir="test_trainer", evaluation_strategy="epoch")

# 创建Trainer对象并进行微调
trainer = Trainer(
    model=model,
    args=training_args,
    train_dataset=small_train_dataset,
    eval_dataset=small_eval_dataset,
    compute_metrics=compute_metrics,
)

trainer.train()
```

## 在原生PyTorch中训练

```python
from datasets import load_dataset
from transformers import AutoTokenizer
from torch.utils.data import DataLoader
from transformers import AutoModelForSequenceClassification
from torch.optim import AdamW
from transformers import get_scheduler
import torch
from tqdm.auto import tqdm
import evaluate


dataset = load_dataset("E:\\huggingface\\datasets_modules\\yelp_review_full", cache_dir="E:\\huggingface\\datasets")

tokenizer = AutoTokenizer.from_pretrained("E:\\huggingface\\bert-base-cased", model_max_length=322)

def tokenize_function(examples):
    return tokenizer(examples["text"], padding="max_length", truncation=True)

tokenized_datasets = dataset.map(tokenize_function, batched=True)

# 移除text列，因为模型不接受原始文本作为输入
tokenized_datasets = tokenized_datasets.remove_columns(["text"])
# 将label列重命名为labels，因为模型期望参数的名称为labels
tokenized_datasets = tokenized_datasets.rename_column("label", "labels")
# 设置数据集的格式以返回PyTorch张量而不是lists
tokenized_datasets.set_format("torch")

# 创建一个较小的子集
small_train_dataset = tokenized_datasets["train"].shuffle(seed=42).select(range(1000))
small_eval_dataset = tokenized_datasets["test"].shuffle(seed=42).select(range(1000))

# 创建DataLoader类
train_dataloader = DataLoader(small_train_dataset, shuffle=True, batch_size=8)
eval_dataloader = DataLoader(small_eval_dataset, batch_size=8)

model = AutoModelForSequenceClassification.from_pretrained("E:\\huggingface\\bert-base-cased", num_labels=5)

# 创建优化器
optimizer = AdamW(model.parameters(), lr=5e-5)

# 创建learning rate scheduler
num_epochs = 3
num_training_steps = num_epochs * len(train_dataloader)
lr_scheduler = get_scheduler(
    name="linear", optimizer=optimizer, num_warmup_steps=0, num_training_steps=num_training_steps
)

device = torch.device("cuda") if torch.cuda.is_available() else torch.device("cpu")
model.to(device)

progress_bar = tqdm(range(num_training_steps))

# 训练
model.train()
for epoch in range(num_epochs):
    for batch in train_dataloader:
        batch = {k: v.to(device) for k, v in batch.items()}
        outputs = model(**batch)
        loss = outputs.loss
        loss.backward()

        optimizer.step()
        lr_scheduler.step()
        optimizer.zero_grad()
        progress_bar.update(1)

# 评估
metric = evaluate.load("E:\\huggingface\\evaluate\\accuracy")
model.eval()
for batch in eval_dataloader:
    batch = {k: v.to(device) for k, v in batch.items()}
    with torch.no_grad():
        outputs = model(**batch)

    logits = outputs.logits
    predictions = torch.argmax(logits, dim=-1)
    # 使用add_batch累积所有批次
    metric.add_batch(predictions=predictions, references=batch["labels"])

# 最后计算指标
metric.compute()
```

- pytorch中的backward()函数计算梯度时是累积计算而不是替换，在处理每一个batch时并不需要与其他batch的梯度混合起来累积计算，因此处理完一个batch需要调用zero_grad()函数将梯度参数置零。
