# Pub/Sub by PyO3

- src/lib.rs

```rust
use std::collections::HashMap;
use pyo3::prelude::*;

#[pyclass]
#[derive(FromPyObject)]
struct Event {
    #[pyo3(get)]
    pub topic: String,
    #[pyo3(get)]
    pub data: String,
}

#[pymethods]
impl Event {
    #[new]
    pub fn new(topic: &str, data: &str) -> Self {
        Self {
            topic: topic.to_string(),
            data: data.to_string(),
        }
    }
}

#[pyclass]
struct EventEngine {
    handlers: HashMap<String, Py<PyAny>>,
}

#[pymethods]
impl EventEngine {
    #[new]
    pub fn new() -> Self {
        Self {
            handlers: HashMap::new(),
        }
    }

    pub fn subscribe(&mut self, topic: String, py_function: Bound<'_, PyAny>) {
        println!("subscribe: {}", topic);
        self.handlers.insert(topic, py_function.unbind());
    }

    pub fn publish(&mut self, py: Python<'_>, event: Event) -> PyResult<()> {
        println!("publish: {}", event.topic);
        if let Some(handler) = self.handlers.get_mut(&event.topic) {
            let bound_handler = handler.bind(py);
            bound_handler.call1((event,))?;
        }
        Ok(())
    }
}

#[pymodule]
mod event_module {
    use super::*;

    #[pymodule_init]
    fn init(m: &Bound<'_, PyModule>) -> PyResult<()> {
        m.add_class::<Event>()?;
        m.add_class::<EventEngine>()?;
        Ok(())
    }
}
```

- test.py

```python
from event_module import EventEngine, Event

class MessageRobot:
    def __init__(self) -> None:
        self.engine = EventEngine()
    
    def register(self):
        self.engine.subscribe('test_topic', self.on_callback)

    def on_callback(self, evt):
        print('on_callback:', evt.topic, evt.data)

    def on_message(self):
        evt = Event('test_topic', 'this is a message')
        print('on_message:', evt.topic)
        self.engine.publish(evt)


if __name__ == '__main__':
    robot = MessageRobot()
    robot.register()
    robot.on_message()
```