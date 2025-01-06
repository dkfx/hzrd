---
layout: post
title: 'Building a Deep Learning Server: Hardware'
hide_title: false
feature-img: "assets/images/posts/building-ai-server.png"
bootstrap: false
tags: [hardware, ai, how-to]
---
I think that, like many people, my understanding of AI has undergone significant evolution in 2024. Despite my best efforts to keep up with the field, I've come to realize that doing a good job at it is essentially a full-time job in itself. The sheer number of models and tools being released seems to be accelerating, with new research and development emerging on a daily, sometimes hourly, basis.

In an effort to stay current and because I genuinely enjoy learning about new technology, I set out to build a local server that can run some AI workloads, allowing me to explore the capabilities of AI. At this point, I'll be focusing on [LLMs](https://en.wikipedia.org/wiki/Large_language_model), as I believe this area will have the most significant impact on my productivity.

To begin, I conducted research and found numerous posts online about AI rigs built by [Redditors](https://www.reddit.com/r/LocalLLaMA/comments/1g6ixae/6x_gpu_build_4x_rtx_3090_and_2x_mi60_epyc_7002/) and [bloggers](https://ahmadosman.com/blog/serving-ai-from-the-basement-part-i/). They all emphasized the same basic requirements: a decent CPU with plenty of PCI lanes, a motherboard with sufficient PCI slots (and, to some degree, speed) to accommodate multiple GPUs, and ample RAM and disk space to store models and other data.

The clear winner in terms of hardware is an AMD EPYC motherboard and processor setup. However, I was able to find a used [ASRock X99 WS-E](https://www.asrock.com/mb/intel/X99%20WS-E/) motherboard and an [Intel Xeon E5-2680 v4](https://www.intel.com/content/www/us/en/products/sku/91754/intel-xeon-processor-e52680-v4-35m-cache-2-40-ghz/specifications.html) on eBay at a very affordable price, so I decided to go with that configuration. The critical aspect is having enough PCI lanes and ports to support your goals; most modern consumer boards and CPUs lack the necessary expandability. Server motherboards are typically where you need to look, but you can use PCPartPicker to find motherboards with [7+ PCIe x16 slots](https://pcpartpicker.com/products/motherboard/#h=7,8&xcx=0) to get started. Keep in mind that I wanted plenty of room for future growth; if you don't plan on having as many GPUs, you can get by with fewer slots. Be sure to check the speed at which the slots will operate when multiple cards are installed and consider how some boards share bandwidth with storage like NVMe, depending on the slot configuration.

As I'm not an avid gamer, I didn't have any spare video cards lying around. I found a pair of RTX 3090 video cards on eBay, taking advantage of the looming release of the 5xxx series cards and the new administration's promise of stiff tariffs on imports, which has led to fluctuating card prices. So far, I'm very happy with the two cards I purchased. If I had to do it over again (and if I had a choice, considering the used market), I would have paid closer attention to the power requirements of the cards. One of them requires an additional set of power cables (for a total of three sets) to run the card, and when you're already drawing significant power, every bit of savings in both power supply plug and output space matters. Be sure to do your research and beware of scams, as those cheaper cards on eBay often seem too good to be true – they may never arrive at your doorstep.

Regarding the other components, most are relatively inexpensive parts from Amazon or NewEgg. It's essential to pay attention to power requirements, as GPUs are very power-hungry. I started with a single 1300W EVGA PSU, which provides ample power and modular slots for GPU power cables. If I expand the server beyond a few GPUs, I'll need an additional PSU and will have to connect them to deliver sufficient power to the system.

Here are the specs of my server:
- Motherboard: ASRock X99 WS-E
- CPU: Intel E5-2680V4
- GPUs: 2x GeForce RTX 3090 24GB GDDR6X
- RAM: 8x16GB DDR4
- Storage: Silicon Power 2TB NVMe M.2 PCIe Gen3x4 2280 SSD
- PSU: EVGA 1300 GT, 80 Plus Gold 1300W
- Case: 12GPU miner case with some PCIe 4.0 riser cables
- Cooling: CPU air cooler with dual 120mm fans, additional 120mm fans as needed

{% include aligner.html 
  images="/content/ai-server-early.jpg" 
  column=1 
  caption="Early photo of the server build. Note that the CPU cooler needs to be turned." 
%}

As I continue to work on this project, I'll be sharing more details about my progress, including benchmarks and performance metrics. In a future post, I'll delve deeper into the specifics of my setup, discussing the challenges I've faced, the lessons I've learned, and the potential applications. For now, I'm excited to see where this journey takes me and how my server will evolve over time.