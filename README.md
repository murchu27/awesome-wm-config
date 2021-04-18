# AwesomeWM config files

This README documents changes from the default AwesomeWM config that ships with
[Manjaro Awesome edition][1] (which is what I currently run).

## Presentation/Layout

### Layouts
Added the [centerwork layout][5] from [lain][6]

    local lain = require("lain")
    ...
    theme.lain_icons = os.getenv("HOME") .. "/.config/awesome/lain/icons/layout/default/"
    theme.layout_centerwork = theme.lain_icons .. "centerwork.png"
    ...
    awful.layout.layouts = {
        ...
        lain.layout.centerwork,
        ...
    }

This required installing lain [from the AUR][7], and then symlinking the lain
installation directory in `~/.config/awesome` (this symlink should already be
present in this repo).

Also commented out a good few layouts that I don't use

### Wibar
Made the height of the wibar a bit smaller

    s.mywibox = awful.wibar({ position = "top", height = 24, screen = s })

#### Widgets
Added a battery-widget, from [awesome-wm-widgets][4]. Also changed the warning
message that it usually displays for low battery.

    local battery_widget = require("awesome-wm-widgets.battery-widget.battery")
    ...
    s.mywibox:setup {
    ...
        { -- Right widgets
            ...
            battery_widget({warning_msg_title = "Get ya charga"}),
            ...
        }
    ...
    }

This required installing [arc-icon-theme-git][8] and
[moka-icon-theme-git][9] from the AUR.

### Clients
#### Useless gaps
Added a bit of space between clients

    beautiful.useless_gap = 5


#### Titlebars
Removed titlebars (they just take up space)

    awful.rules.rules = {
        ...
        { rule_any = {type = { "normal", "dialog" }
            }, properties = { titlebars_enabled = false }
        },
        ...
    }


#### Floating clients
Added some more floating clients to the default list

    awful.rules.rules = {
        ...
        { rule_any = {
            ...
            class = {
                ...
                "Blueman-manager",
                "Godot_Engine", -- this one's not working yet
                ...
            },
            role = {
                ...
                "ConfigManager",
                ...
            },
            ...
        }, properties = { floating = true }},
        ...
    }


### Font
Made the font a bit smaller, including the font for notifications

    beautiful.font = "Noto Sans Regular 9"
    beautiful.notification_font = "Noto Sans Bold 11"

### Wallpaper
Added some neato wallpapers. Each screen is set based on its resolution (as I
have a regular 16:9 laptop screen, and a 21:9 external monitor)

    beautiful.wallpaper = function(s)
        if s.geometry.width == 1920 then
            return "/home/murchu27/.config/awesome/wallpapers/hyrule.jpg"
        elseif s.geometry.width == 2560 then
            return "/home/murchu27/.config/awesome/wallpapers/summit.png"
        end
    end


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

### Default master width factor
Changed how wide the master client appears by default

    beautiful.master_width_factor = 0.65


## Programs

### Defaults

Changed `gui_editor` (which the default config uses anytime an argument refers
to an editor)

    gui_editor = "gvim"
    
### Shortcuts

Added a shortcut to open gvim

    awful.key({ modkey,           }, "g", function () awful.spawn(gui_editor) end,
              {description = "open gvim", group = "launcher"}),

Added a shortcut to change keyboard layout for when I'm not on my Atreus (needs
work)

    awful.key({ modkey, "Shift"   }, "space", function () awful.util.spawn("setxkbmap us -variant colemak") end,
                {description = "switch kb layout", group = "layout"}),

Added a shortcut for "select next layout", which really should have been there

    awful.key({ modkey,           }, "space", function () awful.layout.inc( 1)                  end,
                {description = "select next", group = "layout"}),

Changed the shortcuts for taking screenshots, as I use a small form factor
keyboard which doesn't have the "Print" button

    awful.key({ modkey, "Control", "Shift"  }, "s", function () awful.spawn.with_shell("sleep 0.1 && /usr/bin/i3-scrot -d")   end,
              {description = "capture a screenshot", group = "screenshot"}),
    awful.key({ modkey, "Control", "Alt"    }, "s", function () awful.spawn.with_shell("sleep 0.1 && /usr/bin/i3-scrot -w")   end,
              {description = "capture a screenshot of active window", group = "screenshot"}),
    awful.key({ modkey, "Control"           }, "s", function () awful.spawn.with_shell("sleep 0.1 && /usr/bin/i3-scrot -s")   end,
              {description = "capture a screenshot of selection", group = "screenshot"}),

Changed the shortcut for the Lua prompt, which uses ö by default (not an option
on my keyboard)

    awful.key({ modkey }, "x",
        function ()
            awful.prompt.run { ... })

This also forced me to change the default shortcut for closing a client

    awful.key({ modkey, "Control" }, "x",      function (c) c:kill()                         end,

 [1]: https://gitlab.manjaro.org/profiles-and-settings/desktop-settings/-/tree/master/community/awesome/skel/.config/awesome
 [2]: https://stackoverflow.com/questions/66687389/changing-default-focused-screen-on-awesome-wm
 [3]: https://github.com/Askannz/optimus-manager#usage
 [4]: https://github.com/streetturtle/awesome-wm-widgets
 [5]: https://github.com/lcpz/lain/wiki/Layouts#centerwork
 [6]: https://github.com/lcpz/lain
 [7]: https://aur.archlinux.org/packages/lain-git
 [8]: https://aur.archlinux.org/packages/arc-icon-theme-git/
 [9]: https://aur.archlinux.org/packages/moka-icon-theme-git/
