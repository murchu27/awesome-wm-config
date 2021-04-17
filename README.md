# AwesomeWM config files

This README documents changes from the default AwesomeWM config that ships with
[Manjaro Awesome edition][1] (which is what I currently run).

## Presentation/Layout

### Font
Made the font a bit smaller

    beautiful.font = "Noto Sans Regular 9"

### Wibar
Made the height of the wibar a bit smaller

    s.mywibox = awful.wibar({ position = "top", height = 24, screen = s })


## Behaviour

### Default mouse location
When awesome starts, the mouse pointer is always placed on the primary screen
(according to xrandr)

    -- Start mouse on primary screen always
    mouse.screen = screen.primary

This is because when I use an external monitor, xrandr configures it as primary
when I login, but awesome doesn't begin focus there by default. This config
sorts that out by forcing the pointer to the primary xrandr screen. See 
[this SO question][2].

Side note: when my laptop is plugged into an external monitor, with my
laptop lid closed, the backlight gets switched off, but my xrandr layout leaves
my laptop screen enabled. This lets optimus-manager stay in [hybrid mode][3],
without causing performance issues (can't find a reference with a quick search,
but try it yourself, turning off the laptop screen in hybrid mode causes lag).

## Programs

### Defaults

Changed `gui_editor` (which the default config uses anytime an argument refers
to an editor)

    gui_editor = "gvim"
    
### Shortcuts

Added a shortcut to open gvim

    awful.key({ modkey,           }, "g", function () awful.spawn(gui_editor) end,
              {description = "open gvim", group = "launcher"}),

Changed the shortcuts for taking screenshots, as I use a small form factor
keyboard which doesn't have the "Print" button

    awful.key({ modkey, "Control", "Shift"  }, "s", function () awful.spawn.with_shell("sleep 0.1 && /usr/bin/i3-scrot -d")   end,
              {description = "capture a screenshot", group = "screenshot"}),
    awful.key({ modkey, "Control", "Alt"    }, "s", function () awful.spawn.with_shell("sleep 0.1 && /usr/bin/i3-scrot -w")   end,
              {description = "capture a screenshot of active window", group = "screenshot"}),
    awful.key({ modkey, "Control"           }, "s", function () awful.spawn.with_shell("sleep 0.1 && /usr/bin/i3-scrot -s")   end,
              {description = "capture a screenshot of selection", group = "screenshot"}),

 [1]: https://gitlab.manjaro.org/profiles-and-settings/desktop-settings/-/tree/master/community/awesome/skel/.config/awesome
 [2]: https://stackoverflow.com/questions/66687389/changing-default-focused-screen-on-awesome-wm
 [3]: https://github.com/Askannz/optimus-manager#usage
