---
title: conky华丽桌面显示系统仪表盘
date: 2019-08-09 11:54:15
tags:
 - conky
 - 软件
categories:
 - 软件
---

conky是在桌面可以显示系统信息的仪表盘，cpu，内存网络，磁盘等等，配置好了，很好看

![conky](conky.png)

```
sudo pacman -S conky conky-manager
```
after conky 1.10, use lua syntax

我的配置，``～/.config/conky/conky.conf``
```
conky.config = {
    alignment = 'top_right',
    
    background = false,
    
    border_width = 1,
    
    cpu_avg_samples = 2,
    net_avg_samples = 2,
    
    use_xft = true,
    -- Xft font when Xft is enabled
    font = 'Sans:size=9',
    -- Text alpha when using Xft
    xftalpha = 0.8,
	
    default_color = 'white',
    default_outline_color = 'white',
    default_shade_color = 'white',
    
    draw_borders = false,
    draw_graph_borders = true,
    draw_outline = false,
    draw_shades = false,

    gap_x = 5,
    gap_y = 31,
    minimum_height = 5,
    minimum_width = 5,
	
    no_buffers = true,
    out_to_console = false,
    out_to_stderr = false,
    extra_newline = false,

    double_buffer = true,
    -- Create own window instead of using desktop (required in nautilus)
    own_window = true,
    own_window_class = 'Conky',
    own_window_argb_visual = true,
    own_window_transparent = true,
    own_window_hints = 'undecorated,below,sticky,skip_taskbar,skip_pager',
    own_window_type = 'desktop',

    stippled_borders = 0,
    update_interval = 1.0,
    uppercase = false,
    use_spacer = 'none',
    show_graph_scale = false,
    show_graph_range = false
}

conky.text = [[
${alignc}${color4}${time %a} ${time %b} ${time %e}     ${alignc}${color1}${time %H}:${color2}${time %M}:${color3}${time %S}
${color white}SYSTEM ${hr 1}${color}
Hostname: $alignr$nodename
Kernel: $alignr$kernel
Uptime: $alignr$uptime

CPU: ${alignr}${freq dyn} MHz
Processes: ${alignr}$processes ($running_processes running)
Load: ${alignr}$loadavg

CPU ${alignr}${cpu cpu0}%
${cpubar 4 cpu0}
Ram ${alignr}$mem / $memmax ($memperc%)
${membar 4}
swap ${alignr}$swap / $swapmax ($swapperc%)
${swapbar 4}

Highest CPU $alignr CPU%  MEM%
${top name 1}$alignr${top cpu 1}   ${top mem 1}
${top name 2}$alignr${top cpu 2}   ${top mem 2}
${top name 3}$alignr${top cpu 3}   ${top mem 3}

Highest MEM $alignr CPU%  MEM%
${top_mem name 1}$alignr${top_mem cpu 1}   ${top_mem mem 1}
${top_mem name 2}$alignr${top_mem cpu 2}   ${top_mem mem 2}
${top_mem name 3}$alignr${top_mem cpu 3}   ${top_mem mem 3}

${color white}NETWORK ${hr 1}${color}
Down ${downspeed wlan0}/s ${alignr}Up ${upspeed wlan0}/s
${downspeedgraph wlan0 25,107} ${alignr}${upspeedgraph wlan0 25,107}
Total ${totaldown wlan0} ${alignr}Total ${totalup wlan0}

${color white}DISKIO ${hr 1}${color}
Read ${diskio_read}/s ${alignr}Write ${diskio_write}/s
${diskiograph_read /dev/sda 25,107} ${alignr}${diskiograph_read /dev/sda 25,107}
]]
```