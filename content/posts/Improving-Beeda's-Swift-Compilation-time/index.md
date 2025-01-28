---
title: "Improving Beeda's Swift Compilation times by 83%"
date: 2023-02-09T08:01:47+06:00
description: Beeda achieved a remarkable 83% reduction in Swift compilation times, optimizing development workflows. This enhancement boosts efficiency, saving valuable time for developers.
menu:
  sidebar:
    name: Improving Beeda's Swift Compilation times by 83%
    identifier: bc
    weight: 3
tags: ["Beeda", "Optimisation"]
categories: ["basic"]
---

## Context

As Beeda expanded with new features, compile times began to pose a serious challenge. A simple change or full compilation took around 11 minutes, slowing down our workflow significantly.  

To enhance productivity and streamline development, we aimed to drastically reduce this compile time. In this article, I’ll walk you through how we successfully brought it down to just 2 minutes.  

## Optimization Levels in Xcode  

Xcode provides three optimization levels to choose from:  
- **None**  
- **Fast**  
- **Fast with Whole Module Optimization**  

Here’s how we leveraged these settings to achieve this improvement. 

![image info](/images/site/bc.jpg)

Enabling Whole Module Optimization greatly accelerates the compilation process. However, selecting the Fast or Fast with Whole Module Optimization settings disables debugging functionality. After choosing one of these options and compiling the app, attempting to debug results in the following message in the console:

> App was compiled with optimization - stepping may behave oddly; variables may not be available.



---

## Solution: Add a User-Defined Setting
To enable full module optimization, you’ll need to manually add a **User-Defined Setting** in your Xcode project configuration. Here’s how you can do it:

---

### Steps



1. **Navigate to Project Settings**:
   - In the Project Navigator, select your project.
   - Go to the **Build Settings** tab.

2. **Add a User-Defined Setting**:

   ![image info](/images/site/ol.jpg)
   ---

   ![image info](/images/site/mo.jpg)

   - Click the **+** button in the top-left corner of the Build Settings pane.
   - Select **Add User-Defined Setting**.
   - Name the setting `SWIFT_WHOLE_OPTIMIZATION_LEVEL` (or another relevant name).
   - Set its value to `YES` to enable full module optimization.
   ---
3. **Set None in Debug configuration**
   - From target build settings set Optimization Level to None in Debug configuration



### How Does This Impact Beeda?

For our iOS team, which frequently updates and iterates on our code, this enhancement plays a significant role in improving **efficiency**, **reducing costs**, and **enhancing the customer experience**. To put it into perspective, if we run 30 compilations each day, this optimization saves us roughly **26 hours of compilation time per day**. This time savings is akin to having the output of **three extra developers** on the team.
