# Tokio中的四种Channel

1. Oneshot Channel: single-producer, single-consumer

```rust
use tokio::sync::oneshot;
use tokio::sync::oneshot::{Receiver, Sender};

async fn spy(tx: Sender<String>, code: i32) {
    tx.send(format!("spy message: {code}")).unwrap();
}

async fn double_agent(mut rx_1: Receiver<String>, mut rx_2: Receiver<String>) {
    let msg = tokio::select! {
        msg_1 = &mut rx_1 => msg_1.unwrap(),
        msg_2 = &mut rx_2 => msg_2.unwrap(),
    };
    println!("Spy has received the following msg: {}", msg);
}

#[tokio::main]
async fn main() {
    let (tx1, rx1) = oneshot::channel();
    let (tx2, rx2) = oneshot::channel();

    let handle_1 = tokio::spawn(spy(tx1, 1));
    let handle_2 = tokio::spawn(spy(tx2, 2));
    let handle_3 = tokio::spawn(double_agent(rx1, rx2));

    handle_1.await.unwrap();
    handle_2.await.unwrap();
    handle_3.await.unwrap();
}
```

2. MPSC Channel: Multiple-producer, single-consumer

```rust
use std::sync::Arc;
use std::time::Duration;
use tokio::sync::mpsc;
use tokio::time::sleep;

async fn student(id: i32, tx: Arc<mpsc::Sender<String>>) {
    println!("Student {} is getting their homework", id);
    sleep(Duration::from_secs(2)).await;
    tx.send(format!("Student {id}'s homework")).await.unwrap();
}

async fn teacher(mut rx: mpsc::Receiver<String>) -> Vec<String> {
    let mut homework = Vec::new();

    while let Some(student_hw) = rx.recv().await {
        println!("Received homework: {}", &student_hw);
        sleep(Duration::from_secs(1)).await;
        homework.push(student_hw);
    }

    homework
}

#[tokio::main]
async fn main() {
    let (tx, rx) = mpsc::channel(100);
    let tx_arc = Arc::new(tx);

    let teacher_handle = tokio::spawn(teacher(rx));

    let mut student_handles = Vec::new();
    for num in 0..10 {
        student_handles.push(tokio::spawn(student(num, tx_arc.clone())));
    }
    drop(tx_arc);

    for handle in student_handles {
        handle.await.unwrap();
    }
    let result = teacher_handle.await.unwrap();
    println!("teacher result: {:?}", result);
}
```

3. Watch Channel: single-producer, multiple-consumer

```rust
use tokio::sync::watch;

#[derive(Debug, Clone)]
struct Config {
    db: String,
}

impl Config {
    fn new(db: String) -> Config {
        Self { db }
    }

    fn update(&mut self, db: String) {
        self.db = db;
    }
}

async fn listens_for_change(mut rx: watch::Receiver<Config>) {
    while rx.changed().await.is_ok() {
        let new_config = rx.borrow().clone();
        println!("New config: {:?}", new_config);
    }
}

#[tokio::main]
async fn main() {
    let mut config = Config::new("db_path".to_string());

    let (tx, rx_1) = watch::channel(config.clone());
    let rx_2 = tx.subscribe();

    let handle_1 = tokio::spawn(listens_for_change(rx_1));
    let handle_2 = tokio::spawn(listens_for_change(rx_2));

    config.update("new_db_path".to_string());
    tx.send(config.clone()).unwrap();

    // Closing the channel
    drop(tx);

    handle_1.await.unwrap();
    handle_2.await.unwrap();
}
```

4. Broadcast Channel: multiple-producer, multiple-consumer

```rust
use tokio::sync::broadcast;
use tokio::sync::broadcast::{Receiver, Sender};

async fn publisher(tx: Sender<String>, msg: String) {
    tx.send(msg).unwrap();
}

async fn subscriber(mut rx_1: Receiver<String>, mut rx_2: Receiver<String>, mut rx_3: Receiver<String>) {
    loop {
        tokio::select! {
            msg_1 = rx_1.recv() => {
                println!("Receiver 1 from: {}", msg_1.unwrap());
            }
            msg_2 = rx_2.recv() => {
                println!("Receiver 2 from: {}", msg_2.unwrap());
            }
            msg_3 = rx_3.recv() => {
                println!("Receiver 3 from: {}", msg_3.unwrap());
            }
        }
    }
}

#[tokio::main]
async fn main() {
    let (tx, _rx) = broadcast::channel(100);

    let tx_clone_1 = tx.clone();
    let tx_clone_2 = tx_clone_1.clone();
    let tx_clone_3 = tx_clone_1.clone();

    let rx_clone_1 = tx_clone_1.subscribe();
    let rx_clone_2 = tx_clone_2.subscribe();
    let rx_clone_3 = tx_clone_3.subscribe();

    let tx_handle_1 = tokio::spawn(publisher(tx_clone_1, format!("Publisher 1")));
    let tx_handle_2 = tokio::spawn(publisher(tx_clone_2, format!("Publisher 2")));
    let tx_handle_3 = tokio::spawn(publisher(tx_clone_3, format!("Publisher 3")));

    let rx_handles = tokio::spawn(subscriber(rx_clone_1, rx_clone_2, rx_clone_3));

    tx_handle_1.await.unwrap();
    tx_handle_2.await.unwrap();
    tx_handle_3.await.unwrap();
    rx_handles.await.unwrap();
    println!("Finished");
}
```