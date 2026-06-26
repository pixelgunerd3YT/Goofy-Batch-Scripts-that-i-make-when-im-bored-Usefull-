import os
import sys
import subprocess

if sys.platform == "win32":
    try:
        import curses
    except ImportError:
        print("[!] Please wait....")
        subprocess.run(["pip", "install", "windows-curses"], stdout=subprocess.DEVNULL)
        import curses
else:
    import curses

SOFTWARE_LIST = [
    ("Google Chrome", "Google.Chrome", "google-chrome-stable", "google-chrome"),
    ("Mozilla Firefox", "Mozilla.Firefox", "firefox", "firefox"),
    ("VLC Media Player", "VideoLAN.VLC", "vlc", "vlc"),
    ("7-Zip / Keka", "7zip.7zip", "p7zip-full", "keka"),
    ("VS Code", "Microsoft.VisualStudioCode", "code", "visual-studio-code"),
    ("Notepad++ / Notepadnext", "Notepad++.Notepad++", "notepadnext", "notepad-next"),
    ("Git", "Git.Git", "git", "git"),
    ("Discord", "Discord.Discord", "discord", "discord"),
    ("Steam", "Valve.Steam", "steam", "steam"),
    ("OBS Studio", "OBSProject.OBSStudio", "obs-studio", "obs"),
    ("Spotify", "Spotify.Spotify", "spotify-client", "spotify"),
]

def OS_Selection_Menu(stdscr):
    curses.curs_set(0)
    stdscr.nodelay(0)
    stdscr.keypad(True)
    
    curses.init_pair(1, curses.COLOR_GREEN, curses.COLOR_BLACK)
    curses.init_pair(2, curses.COLOR_BLACK, curses.COLOR_GREEN)
    
    options = ["Windows (Winget)", "Linux Ubuntu/Debian (APT)", "macOS (Homebrew)"]
    current_row = 0
    
    while True:
        stdscr.clear()
        stdscr.attron(curses.color_pair(1))
        stdscr.addstr("==================================================\n")
        stdscr.addstr("       SELECT YOUR OPERATING SYSTEM\n")
        stdscr.addstr("==================================================\n\n")
        
        for idx, option in enumerate(options):
            if idx == current_row:
                stdscr.attron(curses.color_pair(2))
                stdscr.addstr(f" ->  {option} \n")
                stdscr.attroff(curses.color_pair(2))
                stdscr.attron(curses.color_pair(1))
            else:
                stdscr.addstr(f"     {option} \n")
                
        stdscr.addstr("\n==================================================\n")
        stdscr.refresh()
        
        key = stdscr.getch()
        if key == curses.KEY_UP and current_row > 0:
            current_row -= 1
        elif key == curses.KEY_DOWN and current_row < len(options) - 1:
            current_row += 1
        elif key in [10, 13]:
            return current_row

def Software_Selection_Menu(stdscr, os_choice):
    selected_indices = set()
    current_row = 0
    
    while True:
        stdscr.clear()
        stdscr.attron(curses.color_pair(1))
        stdscr.addstr("==================================================\n")
        stdscr.addstr(" Use ARROWS to move, SPACE to select, ENTER to install\n")
        stdscr.addstr("==================================================\n\n")
        
        for idx, (name, _, _, _) in enumerate(SOFTWARE_LIST):
            pointer = "-> " if idx == current_row else "   "
            marker = "*" if idx in selected_indices else " "
            line = f"{pointer}[{marker}] {name}"
            
            if idx == current_row:
                stdscr.attron(curses.color_pair(2))
                stdscr.addstr(f"{line:<40}\n")
                stdscr.attroff(curses.color_pair(2))
                stdscr.attron(curses.color_pair(1))
            else:
                stdscr.addstr(f"{line}\n")
                
        stdscr.addstr("\n==================================================\n")
        stdscr.refresh()
        
        key = stdscr.getch()
        if key == curses.KEY_UP and current_row > 0:
            current_row -= 1
        elif key == curses.KEY_DOWN and current_row < len(SOFTWARE_LIST) - 1:
            current_row += 1
        elif key == ord(' '): 
            if current_row in selected_indices:
                selected_indices.remove(current_row)
            else:
                selected_indices.add(current_row)
        elif key in [10, 13]: 
            return list(selected_indices)

def main():

    os_choice = curses.wrapper(OS_Selection_Menu)
    chosen_apps = curses.wrapper(Software_Selection_Menu, os_choice)
    
    if not chosen_apps:
        print("\n[!] No apps selected. Exiting.")
        return
# Stop Reading our comments
    print("\n" + "="*50)
    print(" INITIALIZING SELECTIONS... PLEASE DO NOT CLOSE")
    print("="*50 + "\n")

    for idx in chosen_apps:
        app_name, win_id, linux_id, mac_id = SOFTWARE_LIST[idx]
        print(f"[+] Installing {app_name}...")

        if os_choice == 0: 
            subprocess.run(["winget", "install", "-e", "--id", win_id, "--silent", "--accept-source-agreements", "--accept-package-agreements"])
        
        elif os_choice == 1: 
            if app_name == "Google Chrome":
                subprocess.run("wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb -O /tmp/chrome.deb && sudo apt install /tmp/chrome.deb -y", shell=True)
            else:
                subprocess.run(["sudo", "apt", "install", linux_id, "-y"])
                
        elif os_choice == 2:
            subprocess.run(["brew", "install", "--cask", mac_id])

    print("\n" + "="*50)
    print(" ALL INSTALLATIONS COMPLETE!")
    print("="*50)
    input("\nPress Enter to exit...")

if __name__ == "__main__":
    main()
# Why are y'all reading the comments?
