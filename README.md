<img src="logo.png" alt="Bolo game logo"/>

# PyBolo
An enhanced, modern, cross-platform implementation of the classic multiplayer tank game, [Bolo](https://en.wikipedia.org/wiki/Bolo_(1987_video_game)).

Download the latest [release](https://github.com/joncox123/PyBoloPublic/releases) and check out the **[tracker website](http://pybolo.com)**.

### Current Status
**July 22, 2024:** working on a standard tournament bot that will be used on the server to bootstrap multiplayer games. Thus far, I have implemented a C++ Python module that provides a custom A* bidirectional path planning (routing) algorithm. At the moment, I am creating the bot client and adding the logic.

### License information:
PyBolo © Copyright 2024 Jonathan Cox, all rights reserved.

The following portions are subject to a different copyright:
- Original graphics, sounds, map format and Everard Island are © 1987-1995 Stuart Cheshire. Used under permission.
- Map logic code adapted from WinBolo's screencalc.c, © 1999-2008 John Morrison. Adapted with permission under MIT license.
- Pyqtree, © 2018 Karim Bahgat and used under MIT license.

<img src="PyBoloDemo.gif" alt="Demo Video"/>

# README and FAQ
## PyBolo 0.99.1 (beta release 2)

## How can I test PyBolo?
If there is an active server running on the Tracker, you can simply open the PyBolo app and select the server from the list. However, you will need at least one other player to join before you can play. 

If you just want to test it out by yourself, I suggest the following procedure. First, unzip two separate copies of PyBolo into separate directories. Launch the Server app from one of them. Then launch the PyBolo client app from one directory using the command line with: PyBolo.exe pybolo://127.0.0.1:60805, where the syntax is <app path> pybolo://<hostname>:<port> to connect to. Do this as well for PyBolo in the other directory.

Alternatively, you could open two copies of the app and connect both to the server on the Tracker, instead of running your own server. This is equivalent to the practice mode in Bolo.

## How can I submit bug or feature requests?
Please create new "Issues" on this GitHub page. Keep in mind, I will read all of the feature requests, but I probably won't implement many of them because the goals are mainly: (1) simplicity; (2) stability; (3) accessibility to new and experienced players; (4) deep learning AI bots!

## How can I chat in PyBolo, especially in the lobby?
Chat in PyBolo works like Doom or Quake. You can chat in the lobby, after the game, or during the game. Pressing the "T" key will allow you to enter a chat for all players to see. Pressing "Y" will only send a chat to players on your team. Press ENTER to send the chat, or ESC to cancel. 

## How can I change the default settings?
To set the key configuration, activate the menu in the game and click the Set Keys button. Hit the back arrow in the upper right to save your changes.
However, to change other settings please open the following JSON file in a text editor, such as Notepad or similar:
  ./_internal/PyBoloConfig.json

## How do you win a game in PyBolo?
One of the long standing issues with Bolo is that it never offered an end-of-game state. This may be fine for closely knit groups or LAN parties, but it's a practical problem for modern play over the internet amongst people who don't necessarily know the game and each other well. As a result, it is currently possible to win in PyBolo by either taking control of all of the bases or pillboxes. Think of it like checkmate in chess, so protect your bases and pills.

## How can I host my own server?
To host a public server, your router needs to be configured to forward the server's listening port to the machine running your server. While PyBolo has support for UPnP to do this automatically, it sometimes doesn't work properly because UPnP is often implemented very poorly in home routers. If it does work, you will see a message in the server's console indicating that it found your router, the name of the router, your public IP address (WAN) and that a port was opened.

Regardless, the best method for port forwarding is to configure your router for "virtual server" or port forwarding by setting it to forward the TCP port used in the settings to the IP address of the machine running the server. For more information about how to configure your router, please [see this guide.](https://www.noip.com/support/knowledgebase/general-port-forwarding-guide) The default port is 60805, over TCP. You should also give your machine a static IP on your network so that you don't need to reconfigure this setting.

## Which operating systems can PyBolo run on?
PyBolo is written in Python, so it can run on almost any system, in principle. That said, I've tested it thus far on: Windows 10, Windows 11, macOS Big Sur (Intel), macOS Sonoma (arm64, Apple chip), Linux (Ubuntu 22.04 x86) and Android 14 on a Pixel 8 Pro. While iOS is theoretically possible, it will take more work and some of the tools (e.g. kivy) seem to have some issues currently. 

## PyBolo sucks, it's not exactly like Macintosh Bolo from 1993
The intent of PyBolo is to create a modernized, enhanced version of Bolo for the 21st century. It will not be exactly like the original Bolo, but pretty close. So if you want to relive 1993, download the Basilisk II classic Macintosh emulator and install Bolo 0.99.7. However, you might be one of the only people playing the game. If 1993 worked for 2024, Bolo would be much more popular today. Despite that, Bolo is fundamentally an excellent game, it just needs certain updates to be widely embraced again.

The design principle behind PyBolo is to create a desktop and mobile friendly app that is more accessible to novice and experienced users alike. Critical new features include: a lobby with chat, improved in-game chat, assisted team formation (instead of the complicated alliance system of old), automated, team-based intelligent starting positions, delayed spectator mode, real-time minimap with position indicator, mobile friendly user interface, additional sound effects, high-quality upscaling/zoom, end-game detection with winner/loser, after-game next-map voting, automated game "restart" to keep the action going, improved chat interface.

## What is the current status of PyBolo?
PyBolo is mostly feature complete with Bolo, except for a few minor things, but it is perfectly playable, aside from a few bugs or crashes that will be resolved during testing.

## Which features do you plan to implement next?
First, I need to get the remaining bugs fixed. This includes reducing the timing jitter and drift of program execution, so that the clients remain in sync with the server for longer. Second, I need to enhance the graphics drawing performance, as it is very inefficient as currently implemented--redrawing literally everything at every frame (except for the base map). Third, I need to add a tutorial sequence when the app is first launched for people who don't like reading READMEs. Fourth, I need to add the ability to change the key settings in the game's menu. Fifth, I need to improve the game's playability under high lag situations. Finally, I plan to train new AI bots using modern deep-Q reinforcement learning. This way, if you want to play 4x4, for example, but only have 6 players, you could make up the difference with a couple of human skill level bots. Having said all of that, I'll probably work mostly on the bots first since that's the most interesting.

## How does PyBolo differ from Macintosh Bolo or WinBolo, especially under-the-hood?
The networking architecture in PyBolo is experimental and completely different from how either Bolo or WinBolo work. In particular, the game client only transmits your key or mouse inputs to the server, literally nothing else. The server and each client independently run their own asynchronous game engines for all 16 players. The server sends out state updates either periodically or when changes occur to keep everything in sync.

This networking architecture has some significant ramifications. First, it helps prevent cheating because, even if you hack your client, the best you can do is send inputs to the server. You can't teleport across the map, tell the server you are moving 10x as far, have more ammo than you really do, shoot faster, etc. All the server will listen to is "I'm holding down the FIRE and TURN LEFT keys at this moment". 

Second, this network architecture depends critically on accurate estimation of the network delay between your client and the server, as well as the rate of your computer's clock being the same as the server's. In theory, if the estimate of lag is accurate, your game will be smooth and playable, even if the lag is very great. But if it is inaccurate or the lag is highly variable, your game could be choppy. Also, if your computer's clock does not tick at the same rate as the server's your game can evolve differently. In practice, this is generally due to differences or variations in the rate of program execution, not necessarily the quartz oscillator on your motherboard.

Third, your game and the server's "ground truth" can get out of sync. When this happens, your client is responsible for updating its state based on the differential corrections provided by the server. Because of this, you might see certain changes actually get undone and then reconstructed into a new reality before your eyes. For example, a pillbox can spring back to life or a map tile can change back, or a mine can "unexplode". While this seems very strange at first, I would argue it is the best and fairest way. After all, each client has different lag/delay, and the only fair and unbiased umpire is the server. Relativity, my friend. It applies to physics as well as computer games.

Fourth, unlike both Macintosh Bolo and WinBolo, no matter how much any particular client is lagging, it cannot degrade the game for other players. This is another major benefit of this approach.

Beyond the networking architecture, the graphics architecture is also improved. First, PyBolo supports high-quality upscaling, such as the hqx or xbr algorithms. This allows it to look nice at modern resolutions. Also, some minor limitations, such as the 16 fixed sprite rotations are replaced by continuous rotation. There is also a real-time mini-map, which is designed to replace the Map Overview from Classic Macintosh Bolo (and was completely lacking from WinBolo). 

The chat interface is also a drastic improvement over the original Bolo. It shows up to 7 lines and chat is much easier and quicker to enter without taking your eyes off the screen. I decided to eliminate the Newswire messages in favor of adding additional sound effects, so the chat doesn't get cluttered up with messages.

## How did you make PyBolo and is it open source?
Development of PyBolo began on April 12, 2024. PyBolo is 98% written from the ground up exclusively in Python 3, but obviously inspired by the original Bolo by Stuart Cheshire from 1987-1995, under permission. The server reads the original Bolo map format based on the source code from Stuart Cheshire's Bolo that was made public in the early 1990s. However, the map format used to transmit from the server to the clients is new, essentially a bunch of Numpy arrays that are gzip compressed (more efficient than the original Bolo format, and simpler to implement these days). The code that determines the map tiles was adapted from John Morrison's WinBolo screencalc.c under permission with the MIT license. It also uses the pyqtree library for the spatial index, which is provided under the MIT license. 

At the present time, PyBolo is copyrighted by Jonathan Cox, all rights reserved and the code is not open source. That said, I intend to eventually release the code as open source once I figure out the path forward. In particular, hosting servers, especially if running AI bots, and publishing mobile apps costs money and I need to figure out how to make that work before releasing the source.
