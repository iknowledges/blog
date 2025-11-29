# Whisper使用教程

## 安装

1. 安装pytorch和whisper

```
pip install torch --index-url https://download.pytorch.org/whl/cu126
pip install -U openai-whisper
```

2. 安装ffmpeg

```
sudo apt install ffmpeg
```

## python使用

`load_model`会将模型下载到`~/.cache/whisper`目录下，如果下载过慢，可以先从[Whisper模型对应官方地址](https://zhuanlan.zhihu.com/p/1974225459907666863)下载好模型文件，然后拷贝到这个目录下。

```python
from pathlib import Path
import argparse
import whisper


def audio_to_speech(directory, lang):
    """
    directory: path to audio
    lang: en zh
    """
    p = Path(directory)
    model = whisper.load_model("large")
    for child in p.iterdir():
        if child.suffix in [".mp4", ".avi"]:
            result = model.transcribe(child.as_posix(), language=lang, verbose=True)
            save_file = child.with_suffix('.srt')
            save_srt(save_file, result["segments"])

def format_seconds(seconds):
    """
    seconds to H:M:S:M
    """
    hours = str(int(seconds // 3600))
    minutes = str(int((seconds % 3600) // 60))
    seconds = seconds % 60
    milliseconds = str(int((seconds - int(seconds)) * 1000))
    seconds = str(int(seconds))
    if len(hours) < 2:
        hours = '0' + hours
    if len(minutes) < 2:
        minutes = '0' + minutes
    if len(seconds) < 2:
        seconds = '0' + seconds
    if len(milliseconds) < 3:
        milliseconds = '0' * (3-len(milliseconds)) + milliseconds
    return f"{hours}:{minutes}:{seconds},{milliseconds}"

def save_srt(save_file, segments):
    with open(save_file, 'w', encoding='utf-8') as f:
        for i in range(len(segments)):
            seg = segments[i]
            start = format_seconds(float(seg["start"]))
            end = format_seconds(float(seg["end"]))
            f.write(f'{i + 1}\n')
            f.write(f'{start} --> {end}\n')
            f.write(f'{seg["text"]}\n')
            f.write('\n')

def main():
    parser = argparse.ArgumentParser(prog='Whisper')
    parser.add_argument('directory')
    parser.add_argument('--lang', default='zh')
    args = parser.parse_args()
    audio_to_speech(args.directory, args.lang)

if __name__ == '__main__':
    main()
```

#### 参考资料

- [whisper](https://github.com/openai/whisper)