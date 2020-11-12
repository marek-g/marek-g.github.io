---
layout: page
title: Rc trait objects
parent: Rust
grand_parent: Programming
---

# Rc trait objects

## Problem

The problem is to have ability to define types that:
- can be used as trait objects (for example in collections with dynamic dispatching)
- use reference counting
- allow to have weak references for trait objects
- type methods have access to reference counted self

It is not really easy to find out how to implement it in Rust. Here is the solution.

## Solution

This is the solution that illustrates the pattern on simple UI's controls definition.

```rust
use std::rc::{Rc,Weak};
use std::cell::RefCell;

// Control - sample type that uses reference counting
trait Control {
    fn handle_event(self_rc: &Rc<RefCell<Self>>);
}

// Control representation as trait object
trait ControlObject {
    fn handle_event(&self);
    
    fn downgrade(&self) -> Box<dyn WeakControlObject>;
}

impl<C: Control + 'static> ControlObject for Rc<RefCell<C>> {
    fn handle_event(&self) {
        Control::handle_event(self);
    }
    
    fn downgrade(&self) -> Box<dyn WeakControlObject> {
        Box::new(Rc::downgrade(&self))
    }
}

// Weak reference to trait object
trait WeakControlObject {
    fn upgrade(&self) -> Option<Box<dyn ControlObject>>;
}

impl<C: Control + 'static> WeakControlObject for Weak<RefCell<C>> {
    fn upgrade(&self) -> Option<Box<dyn ControlObject>> {
        self.upgrade().map(|t| Box::new(t) as Box<dyn ControlObject>)
    }
}

// Label - sample control
pub struct Label {
    pub text: String,
}

impl Label {
    pub fn new(text: String) -> Rc<RefCell<Self>> {
        Rc::new(RefCell::new(Self {
            text
        }))
    }
}

impl Control for Label {
    fn handle_event(self_rc: &Rc<RefCell<Self>>) {
        println!("Label: {}", self_rc.borrow().text);
    }
}

// Sample usage
fn main() {
    // create label control and cast to control object
    let control_object: Box<dyn ControlObject> = Box::new(Label::new("Hello!".to_string()));
    
    // get weak reference to control object
    let weak_control = control_object.downgrade();
    
    // get back strong reference to control object
    if let Some(control_object) = weak_control.upgrade() {
        // call method on the control object
        control_object.handle_event();
    }
}
```
