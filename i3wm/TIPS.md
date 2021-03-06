## Welcome to my i3 Tips    

There's a couple important bugs to fix, some handy additions, and some shilling.

I looked at the [Qubes i3-settings repository](https://github.com/QubesOS/qubes-desktop-linux-i3-settings-qubes), but I couldn't figure out why there were like, 5 variants of the config file instead of just a couple.  There are lines that somehow don't make it to the config file that Qubes-i3 autogenerates.  I'll submit some tickets, but in the meantime...

### Elevator Pitch

Why should you use i3 window manager?  Because it's amazing.  Once you get the hang of it, you wont want to use a regular DE again.  

It's a tiling window manager, meaning it forces windows to share the screenspace, auto filling the entire screen, without overlapping.  Inbuilt quick keys to launch, close, move, and resize windows, intuitive and easy to remember, all user customizable.   Easy switching between workspaces is almost like having multiple monitors.  

It's lighter on system resources than a full DE.  Likely a factor in fixing problems with video playback at full screen and resolution.  You can still run GUI programs, often look and run better.  Your workflow and efficiency will improve, especially if you use my i3gen.sh to load out with quick keys that can launch most any program in any VM with literally just a few keystrokes.

Also, you'll get some hacker cred (not among hackers), but among normies who have never seen such wizardry.  You could hand your machine to your nosy girlfriend, and even if you gave her your passwords, it still wouldn't matter because she wouldn't be able to navigate the damn thing anyways.  Just kidding.  No one who runs i3-Qubes has a girlfriend. 

### Config Changes

**BUGS** 

`i3lock -d` Remove the \`-d' option.  
It's deprecated according to the man page, caused intermittent re-login freezes for me.

Occasionally a pop-up will launch and not show anything.  Toggling fullscreen usually fixes that

&nbsp;   
**CONVENIENT BINDSYMS THAT YOU PROBABLY WANT TO ADD**  
&nbsp;&nbsp;&nbsp; *Note - i3 config file does NOT allow commenting after the end of a line with hashtag \`#'  If you intend to copy-pasta, REMOVE THE COMMENTS!*  

`bindsym $mod+Shift+Return exec konsole` &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	# Launch dom0 terminal (I use Konsole for now)  
`bindsym $mod+m exec qubes-qube-manager`  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;      # Launch QubeManager     
`bindsym $mod+o exec "i3lock -e -f -c 000000"`  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  # Lock screen  
`bindsym $mod+Shift+S exec xfce4-screenshooter`  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; # Launch dom0 screenshot tool  
`bindsym $mod+comma exec pactl set-sink-volume 0 -5%`  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 	# Volume down 5%   
`bindsym $mod+period exec pactl set-sink-volume 0 +5%`  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	# Volume up 5%   
`bindsym $mod+slash exec pactl set-sink-mute 0 toggle`	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	# Mute   

\#\#Too many poorly sized floaters. Force minimum    
`floating_minimum_size 1200 x 800`  

&nbsp;   
**GOTCHAS TO WATCH OUT FOR**

\#\# Qubes-i3 repository has a line to float TorBrowser, but my i3-autoconfig skips it.  
`for_window [title="Tor Browser"] floating enable`  

\#\# Veracrypt was trying to float, much to my chagrin.  
`for_window [title="VeraCrypt"] floating disable`  

\#\# Relates to one of my scripts `qubes-i3-user-command`, I like the popup to float.  
`for_window [title="Run_command_in"] floating enable`

&nbsp;  
**RESIZING WINDOWS**

The default settings is painfully slow to resize windows.  
I increased the defaults, and added some extra keys for fine tune control

```
mode "resize" {   
        # These bindings trigger as soon as you enter the resize mode

        # Pressing left will shrink the windows width.  
        # Pressing right will grow the windows width.  
        # Pressing up will shrink the windows height.  
        # Pressing down will grow the windows height.  

        ### BawdyAnarchist removed '10 ppt' , and changed 10 px to 100 px  
        bindsym j resize shrink width 100 px  
        bindsym k resize grow height 100 px
        bindsym l resize shrink height 100 px
        bindsym semicolon resize grow width 100 px

        ### BawdyAnarchist added new bindings for finer control
        bindsym u resize shrink width 10 px
        bindsym i resize grow height 10 px
        bindsym o resize shrink height 10 px
        bindsym p resize grow width 10 px

        # back to normal: Enter or Escape
        bindsym Return mode "default"
        bindsym Escape mode "default"
}
```


**TOUCHPAD**

You can use `xinput` to enable tapping, but it's not persistent across reboots.  Instead, edit the file `/usr/share/X11/xorg.conf.d/60-libinput.conf`  It wil have a number of sections, find the one that actually has an "Identifier" that says touchpad, and add the two lines you see below.  Obviously set your own mouse/touchpad Accel Speed where you want.

```
Section "InputClass"  
        Identifier "libinput touchpad catchall"  
        MatchIsTouchpad "on"  
        MatchDevicePath "/dev/input/event*"  
        Driver "libinput"  
        Option "Tapping" "True"  
        Option "Accel Speed" "0.7"  
EndSection  
```

&nbsp;  
**xset/dpms**

DPMS manages standby, suspend, and shutdown after defined periods of inactivity.  This can interrupt a lengthy operation (like transfering the Monero and Bitcoin blockchain to a backup drive).  You can use xset to turn this off `xset -dpms` or turn it back on `xset +dpms` or view your current state with `xset q`.  You can also `xset dpms <x y z>` where x y and z are the time in minutes to perform standby, suspend, and shutdown. 

