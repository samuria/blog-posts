## Back story
Back in 2019 I bought a 2008 BMW E92 335i, which I have made considerable changes to since. It was stock car, with not much modifications done to it. And in my ownership, I have done many things like retrofitting seats from a 2018 F80 M3, Manual conversion, turbo swaps etc.

I've always been interested in how the car "works" internally, how does each component communicate with another? These questions led me to research more about car communication protocols, wiring, DME (digital motor electronics) memories and so on. I did lots of research on CANbus and the different modules it can talk to (turns out it talks to everything), I even setup an Arduino with a CANbus shield to talk to a KOMBI (cluster) from a 2008 335i. You can watch a quick demo of that here: [https://youtube.com/shorts/ZoEPezgWxJw](tab:https://youtube.com/shorts/ZoEPezgWxJw)

At some point, I had the idea to collect as much information about the E series Canbus protocol to create my own digital dash. For roll racing, it would have been good to have a dash that could automatically log parameters such as boost pressure, oil pressure, and other drivetrain related information. I was way in over my head at the time, and had 0 clue where to begin. To build plug-in MSD80/81 dash, similar to a Haltech or MaxxECU, is still the goal. I have collected a few spare parts over the years, and I am finally at a point where I can give it a good crack at it. First thing I've decided to do is setup a E90 bench platform, so I can communicate with the different components and learn how they work together.

## Bench setup
**Note:** This will be a learning progress for me, I have decided to document my progess, in case it would help others out in the future - just like how other posts have helped me to learn things I had no clue about. 

### Parts Required
As of writing this post, I do not have my hands on a spare MSD80/81 DME, or the other components for that matter. But I do have a few things from a 2006 E90 325i:

* MSV70 DME
* CAS2 with corresponding key
* JBE
* OBD II port (so we can connect to the computer)
* IHKA (AC controller)
* Clusters from above car + 2008 335i
* All wiring connectors/plugs
* A bunch of other things like seat modules and start buttons
* I have read that I need an ELV emulator if I have a CAS2 module - will research more about this later.

You may not need all of these, this post will mostly cover how to connect the DME, CAS2, and JBE so we can start to communicate and read data from the DME or the CAS. *I warn you again, I am learning as I type this post, and certain things may be incorrect. I will do my best to come back and correct things if I find that I was incorrect.*

![Spare modules that I have](https://github.com/samuria/blog-posts/blob/main/spare-modules.jpg?raw=true)

<center><img src="https://github.com/samuria/blog-posts/blob/main/spare-modules.jpg?raw=true" width="500"></img></center>

### Wiring
I have lots of wire that I pulled from a spare E90, as well as all the OEM plugs for the modules specified above. The plan is to create my own **platform harness**, this harness will facilitate the DME, JBE, CAS and the OBD port to start with. Maybe at a later stage I can add a CCC or CIC into the loom as well.

I have decided to start off with reading lots of existing documentation/research that have already been done by other people, these are some great resources I have found:

* [https://www.spoolstreet.com/threads/e9x-bench-setup.6469/](https://www.spoolstreet.com/threads/e9x-bench-setup.6469/)
* [https://cartechnology.co.uk/printthread.php?tid=40758&page=1](https://cartechnology.co.uk/printthread.php?tid=40758&page=1) - fairly long thread, but great info.
* [https://e46canbus.blogspot.com/2016/12/e90-test-bench.html](https://e46canbus.blogspot.com/2016/12/e90-test-bench.html)
* [https://www.aliexpress.com/i/32978418413.html](https://www.aliexpress.com/i/32978418413.html)- this is a prebuilt test platform harness from aliexpress.
* [https://www.newtis.info/tisv2/a/en/](tab:https://www.newtis.info/tisv2/a/en/) - wiring diagrams for all BMW vehicles. You will need to figure out how to get access, as it is no longer "public".

With all of that in mind, the different combinations of MSV70, MSV80, MSD80, with CAS 2 or CAS3 can all have different connectivity/wiring pins. For now I will only be working with what I have, which is a MSV70 and a CAS2 module.

I will start off by tracing the wiring diagram on newtis, and create my own diagram to demonstrate the harness that I will be making. This is because newtis separates the diagrams into each module, and can be difficult to follow. Having all the modules in one diagram will be helpful to visualise.

![alt text](https://github.com/samuria/blog-posts/blob/main/wiring-v1.png?raw=true)

This is all the wiring that I will need to get the modules powered on. Later on, we can add the other modules (KOMBI, IHKA...) to the mix. But for now, I want to test everything powers on, and I can insert the key. For power, I will be using a 13.8v, 11A power supply. Since the wires on the connectors of each module were cut when they were removed, I will have to solder extra wires, thankfully I have plenty of wire to use.