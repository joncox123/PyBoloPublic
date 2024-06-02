# PyBolo
An enhanced, modern, cross-platform implementation of the classic multiplayer tank game, [Bolo](https://en.wikipedia.org/wiki/Bolo_(1987_video_game)).

Download the latest [release](https://github.com/joncox123/PyBoloPublic/releases) and check out the **[tracker website](http://pybolo.com)**.

### License information:
PyBolo © Copyright 2024 Jonathan Cox, all rights reserved.

The following portions are subject to a different copyright:
- Original graphics, sounds, map format and Everard Island are © 1987-1995 Stuart Cheshire. Used under permission.
- Map logic code adapted from WinBolo's screencalc.c, © 1999-2008 John Morrison. Adapted with permission under MIT license.
- Pyqtree, © 2018 Karim Bahgat and used under MIT license.

<img src="PyBoloDemo.gif" alt="Demo Video"/>

# README and FAQ
## PyBolo 0.99.0 (beta release 1)

## How can I test PyBolo?
If there is an active server running on the Tracker, you can simply open the PyBolo app and select the server from the list. However, you will need at least one other player to join before you can play. 

## How can I submit bug or feature requests?
Please create new "Issues" on this GitHub page. Keep in mind, I will read all of the feature requests, but I probably won't implement many of them because the goals are mainly: (1) simplicity; (2) stability; (3) accessibility to new and experienced players; (4) deep learning AI bots!

If you just want to test it out by yourself, I suggest the following procedure. First, unzip two separate copies of PyBolo into separate directories. Launch the Server app from one of them. Then launch the PyBolo client app from one directory using the command line with: PyBolo.exe pybolo://127.0.0.1:60805, where the syntax is <app path> pybolo://<hostname>:<port> to connec to. Do this as well for PyBolo in the other directory.

## How can I chat in PyBolo, especially in the lobby?
Chat in PyBolo works like Doom or Quake. You can chat in the lobby, after the game, or during the game. Pressing the "T" key will allow you to enter a chat for all players to see. Pressing "Y" will only send a chat to players on your team. Press ENTER to send the chat, or ESC to cancel. 

## How can I change the default settings?
To change certain settings, including the key map, please open the following file in a text editor:
  ./_internal/PyBoloConfig.json
See the end of this README for the key names.

## What operating systems can PyBolo run on?
PyBolo is written in Python, so it can run on almost any system, in principle. That said, I've tested it thus far on: Windows 10, Windows 11, macOS Big Sur (Intel), macOS Sonoma (arm64, Apple chip), Linux (Ubuntu 22.04 x86) and Android 14 on a Pixel 8 Pro. While iOS is theoretically possible, it will take more work and some of the tools (e.g. kivy) seem to have some issues currently. 

## PyBolo sucks, it's not exactly like Macintosh Bolo from 1993 (or WinBolo from 1999)!
The intent of PyBolo is to create a modernized, enhanced version of Bolo for the 21st century. It will not be exactly like the original Bolo, but pretty close. So if you want to relive 1993, download the Basilisk II classic Macintosh emulator and install Bolo 0.99.7. However, you might be the only person on the planet playing the game. If 1993 worked for 2024, Bolo would be much more popular today. Despite that, Bolo is fundamentally an excellent game, it just needs certain updates to be widely embraced again.

The design principle behind PyBolo is to create a dekstop and mobile friendly app that is more accessible to novice and experienced users alike. Critical new features include: a lobby with chat, improved in-game chat, assisted team formation (instead of the complicated alliance system of old), automated, team-based intelligent starting positions, delayed spectator mode, real-time minimap with position indicator, mobile friendly user interface, additional sound effects, high-quality upscaling/zoom, end-game detection with winner/loser, after-game next-map voting, automated game "restart" to keep the action going, improved chat interface.

## What is the current status of PyBolo?
PyBolo is mostly feature complete with Bolo, except for a few minor things, but it is perfectly playable, aside from a few bugs or crashes that will be resolved during testing.

## Which features do you plan to implement next?
First, I need to get the remaining bugs fixed. Second, I need to enhance the graphics drawing performance, as it is very inefficient as currently implemented--redrawing literally everything at every frame (except for the base map). Third, I need to add a tutorial sequence when the app is first launched for people who don't like reading READMEs. Fourth, I need to add the ability to change the key settings in the game's menu. Fifth, I need to improve the game's playability under high lag situations. Finally, I plan to train new AI bots using modern deep-Q reinforcement learning. This way, if you want to play 4x4, for example, but only have 6 players, you could make up the difference with a couple of human skill level bots. Having said all of that, I'll probably work mostly on the bots first since that's the most interesting.

## How does PyBolo differ from Macintosh Bolo or WinBolo, especially under-the-hood?
The networking architecture in PyBolo is experimental and completely different from how either Bolo or WinBolo work. In particular, the game client only transmits your key or mouse inputs to the server, literally nothing else. The server and each client independently run their own asychronous game engines for all 16 players. The server sends out state updates either periodically or when changes occur to keep everything in sync.

This networking architecture has some significant ramifications. First, it helps prevent cheating because, even if your hack your client, the best you can do is send inputs to the server. You can't teleport across the map, tell the server you are moving 10x as far, have more ammo than you really do, shoot faster, etc. All the server will listen to is "I'm holding down the FIRE and TURN LEFT keys at this moment". 

Second, this network architecture depends critically on accurate estimation of the network delay between your client and the server. In theory, if the estimate is accurate, your game will be smooth and playable, even if the lag is very great. But if it is inaccurate or the lag is highly variable, your game could be choppy. 

Third, your game and the server's "ground truth" can get out of sync. When this happens, your client is responsible for updating its state based on the differential corrections provided by the server. Because of this, you might see certain changes actually get undone and then reconstucted into a new reality before your eyes. For example, a pillbox can spring back to life or a map tile can change back, or a mine can "unexplode". While this seems very strange at first, I would argue it is the best and fairest way. After all, each client has different lag/delay, and the only fair and unbiases umpire is the server. Relativity, my friend. It applies to Physics as well as computer games.

Fourth, unlike both Macintosh Bolo and WinBolo, no matter how much any particular client is lagging, it cannot degrade the game for other players. This is another major benefit of this approach.

Beyond the networking architecture, the graphics architecture is also improved. First, PyBolo supports high-quality upscaling, such as the hqx or xbr algorithms. This allows it to look nice at modern resolutions. Also, some minor limitations, such as the 16 fixed sprite rotations are replaced by continuous rotation. There is also a real-time mini-map, which is designed to replace the Map Overview from Classic Macintosh Bolo (and was completely lacking from WinBolo). 

That chat interface is also a drastic improvement over the original Bolo. It shows up to 7 lines and chat is much easier and quicker to enter without taking your eyes off the screen. I decided to eliminate the Newswire messages in favor of adding additional sound effects, so the chat doesn't get cluttered up with messages.

## How did you make PyBolo and is it open source?
Development of PyBolo began on April 12, 2024. PyBolo is 98% written from the ground up exclusively in Python 3, but obviously based on the original Bolo by Stuart Cheshire from 1987-1995, under permission. The server reads the original Bolo map format based on the source code from Stuart Cheshire's Bolo that was made public in the early 1990s. However, the map format used to transmit from the server to the clients is new, essentially a bunch of Numpy arrays that are gzip compressed (more efficient than the original Bolo format, and simpler to implement these days). The code that determines the map tiles was adapted from John Morrison's WinBolo screencalc.c under permission with the MIT license. It also uses the pyqtree library for the spatial index, which is provided under the MIT license. 

At the present time, PyBolo is copyrighted by Jonathan Cox, all rights reserved and the code is not open source. That said, I intend to eventually release the code as open source once I figure out the path forward. In particular, hosting servers, especially if running AI bots, and publishing mobile apps costs money and I need to figure out how to make that work before releasing the source.

## Custom Key Configuration Options

In the PyBoloConfig.json file, you can change the "KEY_MAP" settings to the following possible keys. To open this file on the Mac app, you need to open the app container and find the _internals folder within.

```
https://www.pygame.org/docs/ref/key.html
Constant      ASCII   Description
---------------------------------
K_BACKSPACE   \b      backspace
K_TAB         \t      tab
K_CLEAR               clear
K_RETURN      \r      return
K_PAUSE               pause
K_ESCAPE      ^[      escape
K_SPACE               space
K_EXCLAIM     !       exclaim
K_QUOTEDBL    "       quotedbl
K_HASH        #       hash
K_DOLLAR      $       dollar
K_AMPERSAND   &       ampersand
K_QUOTE               quote
K_LEFTPAREN   (       left parenthesis
K_RIGHTPAREN  )       right parenthesis
K_ASTERISK    *       asterisk
K_PLUS        +       plus sign
K_COMMA       ,       comma
K_MINUS       -       minus sign
K_PERIOD      .       period
K_SLASH       /       forward slash
K_0           0       0
K_1           1       1
K_2           2       2
K_3           3       3
K_4           4       4
K_5           5       5
K_6           6       6
K_7           7       7
K_8           8       8
K_9           9       9
K_COLON       :       colon
K_SEMICOLON   ;       semicolon
K_LESS        <       less-than sign
K_EQUALS      =       equals sign
K_GREATER     >       greater-than sign
K_QUESTION    ?       question mark
K_AT          @       at
K_LEFTBRACKET [       left bracket
K_BACKSLASH   \       backslash
K_RIGHTBRACKET ]      right bracket
K_CARET       ^       caret
K_UNDERSCORE  _       underscore
K_BACKQUOTE   `       grave
K_a           a       a
K_b           b       b
K_c           c       c
K_d           d       d
K_e           e       e
K_f           f       f
K_g           g       g
K_h           h       h
K_i           i       i
K_j           j       j
K_k           k       k
K_l           l       l
K_m           m       m
K_n           n       n
K_o           o       o
K_p           p       p
K_q           q       q
K_r           r       r
K_s           s       s
K_t           t       t
K_u           u       u
K_v           v       v
K_w           w       w
K_x           x       x
K_y           y       y
K_z           z       z
K_DELETE              delete
K_KP0                 keypad 0
K_KP1                 keypad 1
K_KP2                 keypad 2
K_KP3                 keypad 3
K_KP4                 keypad 4
K_KP5                 keypad 5
K_KP6                 keypad 6
K_KP7                 keypad 7
K_KP8                 keypad 8
K_KP9                 keypad 9
K_KP_PERIOD   .       keypad period
K_KP_DIVIDE   /       keypad divide
K_KP_MULTIPLY *       keypad multiply
K_KP_MINUS    -       keypad minus
K_KP_PLUS     +       keypad plus
K_KP_ENTER    \r      keypad enter
K_KP_EQUALS   =       keypad equals
K_UP                  up arrow
K_DOWN                down arrow
K_RIGHT               right arrow
K_LEFT                left arrow
K_INSERT              insert
K_HOME                home
K_END                 end
K_PAGEUP              page up
K_PAGEDOWN            page down
K_F1                  F1
K_F2                  F2
K_F3                  F3
K_F4                  F4
K_F5                  F5
K_F6                  F6
K_F7                  F7
K_F8                  F8
K_F9                  F9
K_F10                 F10
K_F11                 F11
K_F12                 F12
K_F13                 F13
K_F14                 F14
K_F15                 F15
K_NUMLOCK             numlock
K_CAPSLOCK            capslock
K_SCROLLOCK           scrollock
K_RSHIFT              right shift
K_LSHIFT              left shift
K_RCTRL               right control
K_LCTRL               left control
K_RALT                right alt
K_LALT                left alt
K_RMETA               right meta
K_LMETA               left meta
K_LSUPER              left Windows key
K_RSUPER              right Windows key
K_MODE                mode shift
K_HELP                help
K_PRINT               print screen
K_SYSREQ              sysrq
K_BREAK               break
K_MENU                menu
K_POWER               power
K_EURO                Euro
K_AC_BACK             Android back button
```
