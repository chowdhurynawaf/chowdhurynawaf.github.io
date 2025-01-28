---
title: "Optimizing Cache Management Strategy in Beeda"
date: 2024-01-20T08:01:10+06:00
description: Optimizing Cache Management Strategy in Beeda focuses on enhancing performance and user experience by implementing efficient caching mechanisms for media assets like images, Lottie animations, and videos. By leveraging a multi-layered caching approach—including disk caching, LRU caching, and custom solutions—we ensure faster load times, reduced latency, and scalable resource management.
hero: images/site/b1.jpg
menu:
  sidebar:
    name: Cache Management Strategy in Beeda
    identifier: introduction
    weight: 1
tags: ["Beeda", "cache"]
categories: ["basic"]

---

To deliver a fast and seamless user experience, every application requires an effective caching system. Without caching, repeatedly downloading assets can consume significant resources and cause delays in loading times. To overcome this, developers use a variety of caching strategies, including **disk caching**, **LRU (Least Recently Used) caching**, **database caching**, and others, to enhance performance and minimize latency.

---

### Problem:

In the **Beeda User** app, effectively handling media assets like images, Lottie animations, and videos was essential for ensuring a fast and seamless user experience. Each type of media asset comes with its own specific needs and challenges when it comes to caching and optimization:

- **Images**: High-resolution images can consume significant memory and storage, making **disk caching** (e.g., using `URLCache` or third-party libraries like SDWebImage) essential.
- **Lottie Animations**: Lottie files (JSON-based animations) are lightweight but require efficient parsing and rendering. **In-memory caching** (e.g., using `NSCache`) can help reduce repeated parsing overhead.
- **Videos**: Videos are large in size and require **streaming** or **partial caching** to avoid excessive memory and storage usage.

While each caching mechanism is effective for specific media types, a critical question arises: **How can we effectively manage and integrate multiple caching strategies for images, Lottie animations, and videos within an iOS app?**

---

### Implementation:

![image info](/images/site/dg.jpg)

To address the challenges of managing media assets in the **Beeda User** app, we’ve designed different Utils for differnt type accordingly. This structured approach ensures efficient handling of assets like images, Lottie animations, and videos.

### 1. **Utils Layer**
Each type of media asset will have its own **utility class** responsible for handling specific tasks such as downloading, parsing, and processing. These utilities act as intermediaries, fetching assets from the **Assets Manager** without the app directly interacting with it.

For example, consider the **Image Utility**. If an image is not already cached, this utility will download it by calling the `getData` method of the Assets Manager. Once the image is downloaded, the utility will pass the raw data back to the Assets Manager for storage.

> ```swift
> protocol ImageUtilProtocol {
>     func downloadImage(url: String, imageCompletionHandler: ((UIImage) -> Void)?, storageType: StorageType)
> }
> ```

### 2. **Assets Manager**
The **Assets Manager** serves as the **middle layer** in the system, acting as a bridge between the **Utils** and the various **caching mechanisms**. Its primary role is to determine which type of caching should be used based on the specified **storage type**.

The `StorageType` parameter will define the available caching mechanisms (e.g., in-memory, disk, or hybrid). 


> ```swift
> enum StorageType {
>   case lru(LRUCacheType)
>   case disk
> }
> ```

> ```swift
> enum LRUCacheType: String {
>    case lottie
>   case image
>    case video
>}
> ```

The assets manager will have common methods to store and retrieve data.

```swift
protocol AssetsManagerProtocol {
    func writeData(data: Data, forKey key: String, storageType: StorageType)
    func readData(forKey key: String, storageType: StorageType) -> Data?
    func removeData(forKey key: String, storageType: StorageType)
}
```
Assets Manager plays a crucial role in managing caching operations. When the utility classes call its methods, the Assets Manager determines the appropriate caching layer to use based on the storageType. It then delegates the task—whether writing, reading, or removing data—to the corresponding caching mechanism, ensuring efficient and accurate handling of assets.

### 3. **Caching Layer**

This layer contains all the caching methods we use, which includes disk, LRU, database caching. It stores data in its raw form and remains independent of specific data types.

We employ various caching mechanisms depending on the use case. For assets that need to be stored permanently, we rely on disk caching. On the other hand, for temporary data, we utilize the LRU caching method. Additionally, within the LRU caching system, we have separate caches for different types of assets, such as images, Lottie animations, and others.


- ##### **Disk Caching**
Disk caching involves storing data directly on the disk, enabling efficient read and write operations as required.

- ##### **LRU Caching**
Least Recently Used (LRU) caching stores data in memory with a fixed size limit. When the memory reaches its capacity, the least recently accessed item is evicted to accommodate new data.

- ##### **In Our Case:**
  - **For Images**: We utilize both **disk caching** and **LRU caching** to balance performance and storage.
  - **For Rendering Animations**: We rely exclusively on **LRU caching** for faster access and efficient memory usage.
  - **For Rendering Videos**: We employ a **custom disk cache** tailored for handling large video files.

---

![image info](/images/site/uc.jpg)

Each caching layer operates independently and is responsible for its designated task. If we need to introduce a new caching mechanism in the future, it can be seamlessly integrated without disrupting.

---

