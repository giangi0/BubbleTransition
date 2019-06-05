![Swift 5.0](https://img.shields.io/badge/swift-5.0-orange.svg)

A custom modal transition that presents and dismiss a controller inside an expanding and shrinking _bubble_.

# Screenshot
![BubbleTransition](https://raw.githubusercontent.com/andreamazz/BubbleTransition/master/assets/screenshot.gif)

# Usage
Just need to import BubbleTransition.swift into your project.

# Setup
Have your view controller conform to `UIViewControllerTransitioningDelegate`. Set the `transitionMode`, the `startingPoint`, the `bubbleColor` and the `duration`.
```swift
let transition = BubbleTransition()

public override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
  let controller = segue.destination
  controller.transitioningDelegate = self
  controller.modalPresentationStyle = .custom
}

// MARK: UIViewControllerTransitioningDelegate

public func animationController(forPresented presented: UIViewController, presenting: UIViewController, source: UIViewController) -> UIViewControllerAnimatedTransitioning? {
  transition.transitionMode = .present
  transition.startingPoint = someButton.center
  transition.bubbleColor = someButton.backgroundColor!
  return transition
}

public func animationController(forDismissed dismissed: UIViewController) -> UIViewControllerAnimatedTransitioning? {
  transition.transitionMode = .dismiss
  transition.startingPoint = someButton.center
  transition.bubbleColor = someButton.backgroundColor!
  return transition
}
```

# Swipe to dismiss

You can use an interactive gesture to dismiss the presented controller. To enable this gesture, prepare the interactive transition:

```swift
let interactiveTransition = BubbleInteractiveTransition()

override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
  if let controller = segue.destination as? ModalViewController {
    controller.transitioningDelegate = self
    controller.modalPresentationStyle = .custom
    controller.interactiveTransition = interactiveTransition
    interactiveTransition.attach(to: controller)
  }
}
```

and implement `interactionControllerForDismissal` in your presenting controller:

```swift
func interactionControllerForDismissal(using animator: UIViewControllerAnimatedTransitioning) -> UIViewControllerInteractiveTransitioning? {
  return interactiveTransition
}
```

In the presented controller make sure to call `finish()` on the interactive gesture if you need to quickly dismiss from a button press instead. Check the sample code for more info.  

You can decide the gesture threshold and the swipe direction:
```swift
interactiveTransition.interactionThreshold = 0.5
interactionThreshold.swipeDirection = .up
```

# Properties
```swift
var startingPoint = CGPointZero
```
The point that originates the bubble.

```swift
var duration = 0.5
```
The transition duration.

```swift
var transitionMode: BubbleTranisionMode = .present
```
The transition direction. Either `.present`, `.dismiss` or `.pop`.

Checkout the sample project for the full implementation.
