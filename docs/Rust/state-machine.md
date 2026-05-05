# State Machine by Rust

1. Define the state enum and state trigger enum:

```rust
pub enum ComponentTrigger {
    Initialize = 1,
    Start = 2,
    StartCompleted = 3,
    Stop = 4,
    StopCompleted = 5,
    Reset = 6,
    ResetCompleted = 7,
    Fault = 8,
    FaultCompleted = 9,
}

#[derive(Clone, Copy, PartialEq, Eq)]
pub enum ComponentState {
    PreInitialized = 0,
    Ready = 1,
    Starting = 2,
    Running = 3,
    Stopping = 4,
    Stopped = 5,
    Resetting = 6,
    Faulting = 7,
    Faulted = 8,
}

impl ComponentState {
    pub fn transition(&mut self, trigger: &ComponentTrigger) -> anyhow::Result<Self> {
        let new_state = match (&self, trigger) {
            (Self::PreInitialized, ComponentTrigger::Initialize) => Self::Ready,
            (Self::Ready, ComponentTrigger::Reset) => Self::Resetting,
            (Self::Ready, ComponentTrigger::Start) => Self::Starting,
            (Self::Resetting, ComponentTrigger::ResetCompleted) => Self::Ready,
            (Self::Starting, ComponentTrigger::StartCompleted) => Self::Running,
            (Self::Starting, ComponentTrigger::Stop) => Self::Stopping,
            (Self::Starting, ComponentTrigger::Fault) => Self::Faulting,
            (Self::Running, ComponentTrigger::Stop) => Self::Stopping,
            (Self::Running, ComponentTrigger::Fault) => Self::Faulting,
            (Self::Stopping, ComponentTrigger::StopCompleted) => Self::Stopped,
            (Self::Stopping, ComponentTrigger::Fault) => Self::Faulting,
            (Self::Stopped, ComponentTrigger::Reset) => Self::Resetting,
            (Self::Stopped, ComponentTrigger::Fault) => Self::Faulting,
            (Self::Faulting, ComponentTrigger::FaultCompleted) => Self::Faulted,
            _ => anyhow::bail!("Invalid state trigger"),
        };
        Ok(new_state)
    }
}
```

2. Define a trait to change state:

```rust
pub trait Component {
    fn state(&self) -> ComponentState;

    fn is_running(&self) -> bool {
        self.state() == ComponentState::Running
    }

    fn transition_state(&mut self, trigger: ComponentTrigger) -> anyhow::Result<()>;

    fn Initialize(&mut self) -> anyhow::Result<()> {
        self.transition_state(ComponentTrigger::Initialize)
    }

    fn start(&mut self) -> anyhow::Result<()> {
        self.transition_state(ComponentTrigger::Start)?;
        self.on_start()?;
        self.transition_state(ComponentTrigger::StartCompleted)?;
        Ok(())
    }

    fn stop(&mut self) -> anyhow::Result<()> {
        self.transition_state(ComponentTrigger::Stop)?;
        self.on_stop()?;
        self.transition_state(ComponentTrigger::StopCompleted)?;
        Ok(())
    }

    fn reset(&mut self) -> anyhow::Result<()> {
        self.transition_state(ComponentTrigger::Reset)?;
        self.on_reset()?;
        self.transition_state(ComponentTrigger::ResetCompleted)?;
        Ok(())
    }

    fn fault(&mut self) -> anyhow::Result<()> {
        self.transition_state(ComponentTrigger::Fault)?;
        self.on_fault()?;
        self.transition_state(ComponentTrigger::FaultCompleted)?;
        Ok(())
    }

    fn on_start(&mut self) -> anyhow::Result<()> {
        println!("on_start");
        Ok(())
    }

    fn on_stop(&mut self) -> anyhow::Result<()> {
        println!("on_stop");
        Ok(())
    }

    fn on_reset(&mut self) -> anyhow::Result<()> {
        println!("on_reset");
        Ok(())
    }

    fn on_fault(&mut self) -> anyhow::Result<()> {
        println!("on_fault");
        Ok(())
    }
}
```

3. Implement the trait and overwrite the method you needed:

```rust
pub struct TestComponent {
    state: ComponentState,
}

impl TestComponent {
    pub fn new() -> Self {
        Self {
            state: ComponentState::Ready,
        }
    }
}

impl Component for TestComponent {
    fn state(&self) -> ComponentState {
        self.state
    }

    fn transition_state(&mut self, trigger: ComponentTrigger) -> anyhow::Result<()> {
        self.state = self.state.transition(&trigger)?;
        Ok(())
    }

    fn on_start(&mut self) -> anyhow::Result<()> {
        println!("override on_start");
        Ok(())
    }
}
```

4. Test your code:

```rust
fn main() -> anyhow::Result<()> {
    let mut component = TestComponent::new();
    println!("TestComponent is_running: {}", component.is_running());
    component.start()?;
    println!("TestComponent is_running: {}", component.is_running());
    Ok(())
}
```