[return to table](../README.md)

---

# Tmux (Terminal Multiplexier)


## Session

Tmux sessions are persistent, meaning that programs running in Tmux will continue to run even if you get disconnected.
A session can contain multiple windows. Each window works like a separate terminal.

- ```tmux new -s [session_name]```: create a session.
- ```tmux ls```: list the sessions, alternative ```tmux list-sessions```.
- ```tmux list-panes```: list the panes.
- ```tmux a```: attach a session.
- ```tmux attach -t <session_name>```: attach a session by name.
- ```Ctrl```+```B```, ```D``` (detach): return to normal shell terminal.
- ```Ctrl```+```B```, ```:kill-session```: kill the entire session (including its windows).


---

## Window

- ```Ctrl```+```B```, ```C```: create a window.
- ```Ctrl```+```B```, ```:rename-window <window_name>```: rename the current window.
- ```Ctrl```+```B```, ```:kill-window```: kill the current window.
- ```Ctrl```+```B```, ```N```: next window.
- ```Ctrl```+```B```, ```P```: previous window.
- ```Ctrl```+```B```, ```<window_number>```: open window by its number.


---

## Pane

A Pane lives in a window. When the window is killed, its panes are gone.
The purpose of panes is to split the workspace.

- ```Ctrl```+```B```, ```%```: split pane vertically (left/right).
- ```Ctrl```+```B```, ```"```: split pane horizontally (up/down).
- ```Ctrl```+```B```, ```arrow_key```: switch pane.
- Hold ```Ctrl```+```B```, then hold an ```arrow_key```: resize pane.
- ```Ctrl```+```B```, ```X```: kill a pane horizontally (up/down).



---

## Configurations

One can also customize ```tmux``` using its config file ```sudo nano ~/tmux.conf```.

> If the ```~/.tmux.conf``` file doesn’t exist, create it by running the ```touch ~/.tmux.conf``` command.
> Then, open a session, and run ```tmux source-file ~/.tmux.conf```.

### Mouse
- ```Ctrl```+```B```, ```:set -g mouse```: set to use mouse.

### Status bar
- ```Ctrl```+```B```, ```:set -g status-bg cyan```: change the status bar background color.
- ```Ctrl```+```B```, ```:set -g window-status-style bg=yellow```: change inactive window tab color.
- ```Ctrl```+```B```, ```:set -g window-status-current-style bg=yellow```: change active window tab color.

### Pane
- ```Ctrl```+```B```, ```set -g window-active-style bg=colour17```: change the active pane's background color.



### Misc.

- ```Ctrl```+```B```, ```?```: view the key-bindings.




---

## Example tmux Configuration File
```
# Basic
#set-option -g default-shell /usr/bin/zsh
set-option -g default-terminal 'tmux-256color'
set -ga terminal-overrides ',rxvt-unicode-256color:Tc'
set -ga terminal-overrides ",xterm-256color:Tc"
set -ga terminal-overrides ",alacritty:Tc"
set-option -g status-position top
set-option -g status-left-length 20
set-option -g status-right-length 60
set-option -g status-left '#[fg=colour255,bg=colour241]Session: #S #[default]'
set-option -g status-right '#[fg=colour255,bg=colour241] #h | %Y/%m/%d %H:%M:%S#[default]'
set-option -g status-interval 1
set-option -g status-justify centre
set-option -g status-bg "colour238"
set-option -g status-fg "colour255"
set -ag pane-active-border bg=colour111
#set -g window-active-style "bg=colour53,fg=colour46"
#set -g window-style bg=black

# set active-inactive window styles
set -g window-style 'fg=colour247,bg=colour236'
set -g window-active-style 'fg=white,bg=black'

# Mouse
set-option -g mouse on
bind -n WheelUpPane if-shell -F -t = "#{mouse_any_flag}" "send-keys -M" "if -Ft= '#{pane_in_mode}' 'send-keys -M' 'copy-mode -e'"

# Vim like
# select pane
bind h select-pane -L
bind j select-pane -D
bind k select-pane -U
bind l select-pane -R

# resize pane
bind -r H resize-pane -L 5
bind -r J resize-pane -D 5
bind -r K resize-pane -U 5
bind -r L resize-pane -R 5

# copy mode
setw -g mode-keys vi
bind -T copy-mode-vi v send -X begin-selection
bind -T copy-mode-vi V send -X select-line
bind -T copy-mode-vi C-v send -X rectangle-toggle
bind -T copy-mode-vi y send -X copy-selection
bind -T copy-mode-vi Y send -X copy-line
bind-key C-p paste-buffer

# neovim
set-option -sg escape-time 10
set-option -g focus-events on
```









