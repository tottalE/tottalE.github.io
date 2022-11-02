---
layout: post
title: Read this if ScrollView Layout Guide makes you confused.
date: 2022-11-02 17:39:00
description: I will make simple tutorial that you can follow so get ready with Xcode and let`s dive into it! Hands on!
---

In scrollView from iOS 11, apple added scrollView guides but it may make recently first learner confused so I decided to write a short tutorial.

I will make simple tutorial that you can follow so get ready with Xcode and let`s dive into it! Hands on!

### Before we start

First of all, there are two ways you can make scrollViews. **With scrollView guide** and **without scrollView guide**. Lot of tutorials out there are mixing each other so that learners may get confused. So I am going to seperate it so that you can understand what is going. I will only talk with scrollView guide and I will post next one in the future.

I have to explain, that you have to do two things to make scrollView work. **First**, you need to fix the scrollView frame to its superview. **Second**, you need to pin the content subview to the scrollView. It is because you need to set frame as superview size (usally device size). It only shows frame amount of portion of its content size. To be simple, frame is how much you show from you content size.

Download [this](https://github.com/tottalE/scrollViewGuideTutorial/tree/main) example from my repo that you can start hands-on learning. Or you can make with following instructions.

First, make a new project, and go to ViewController.swift file.

In `ViewController.swift` add this code.

- This is contentView that we will put in our scrollView. I added UIVIew with colors so we can reconize it.

```swift
    let contentView: UIStackView = UIStackView()

    // set up stackview to vertical
    private func setupStackView() {
        contentView.translatesAutoresizingMaskIntoConstraints = false
        contentView.axis = .vertical
    }

    // fillstackview with colors.
    private func fillStackView() {
        let colorArray = [UIColor.blue, .red, .yellow, .purple, .green, .black, .orange, .gray]
        for color in colorArray {
            let elementView = UIView()
            elementView.backgroundColor = color
            elementView.translatesAutoresizingMaskIntoConstraints = false
            elementView.heightAnchor.constraint(equalToConstant: 300).isActive = true
            contentView.addArrangedSubview(elementView)
        }
    }
```

- Add this code after. This code creates scrollView and add contentView as its subview.

```swift
private lazy var scrollView: UIScrollView = {
        let scrollView = UIScrollView()
        scrollView.translatesAutoresizingMaskIntoConstraints = false
        return scrollView
    }()
```

- At `viewDidload()` add this code as scrollView become subView of its superView.

```swift
    view.addSubview(scrollView)
    scrollView.addSubview(contentView)
    fillStackView()
```

- make two empty functions that we can learn how to setup scrollViews with two steps.
- **First**, you need to fix the scrollView frame to its superview. **Second**, you need to pin the content subview to the scrollView. So I separted with functions.

```swift
func setupScrollViewToSuperView() {

}

func setupContentViewToScrollView() {

}
```

## With ScrollView guide, new way to do it

### What is layout Guide?

The UIScrollView class has two new layout guide properties from iOS 11:

1. `var frameLayoutGuide: UILayoutGuide { get }`

- We can use it when creating constraints from the scroll view to its superview or when we want to fix (float) a subview of the scroll view over the content. Frame layout guide matches the scroll view frame.

2. `var contentLayoutGuide: UILayoutGuide { get }`

- content layout guide matches the content area of the scroll view so we can use it when constraining our content to the scroll view.

The guides don’t add any new functionality but they do (I think) make it easier to follow what is going on.

### Start Hands-on

Put this code into your `setupScrollViewToSuperView()`

```swift
func setupScrollViewToSuperView() {
    NSLayoutConstraint.activate([
        scrollView.frameLayoutGuide.leadingAnchor.constraint(equalTo: view.leadingAnchor),
        scrollView.frameLayoutGuide.topAnchor.constraint(equalTo: view.topAnchor),
        scrollView.frameLayoutGuide.trailingAnchor.constraint(equalTo: view.trailingAnchor),
        scrollView.frameLayoutGuide.bottomAnchor.constraint(equalTo: view.bottomAnchor),
    ])
}
```

We just made scrollView`s frame size same to view size. So we finished frame setting.

Next, We need to set contentLayoutGuide that constraining how much content we can scroll.

```swift
func setupContentViewToScrollView() {
    NSLayoutConstraint.activate([
        scrollView.contentLayoutGuide.leadingAnchor.constraint(equalTo: contentView.leadingAnchor),
        scrollView.contentLayoutGuide.topAnchor.constraint(equalTo: contentView.topAnchor),
        scrollView.contentLayoutGuide.trailingAnchor.constraint(equalTo: contentView.trailingAnchor),
        scrollView.contentLayoutGuide.bottomAnchor.constraint(equalTo: contentView.bottomAnchor)
    ])

    scrollView.frameLayoutGuide.widthAnchor.constraint(equalTo: scrollView.contentLayoutGuide.widthAnchor)
.isActive = true

}
```

You would understand that we made content size and stackview(contentView)size same that we can set contentsize. But what is last line of code?
`scrollView.frameLayoutGuide.widthAnchor.constraint(equalTo: scrollView.contentLayoutGuide.widthAnchor)`
This means we are constraing content`s width same with our frame that we can scroll only vertically.

full compeleted code is [here](https://github.com/tottalE/scrollViewGuideTutorial/blob/completed/scrollViewGuide/ViewController.swift)

ref: https://useyourloaf.com/blog/easier-scrolling-with-layout-guides/
