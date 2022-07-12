# claim_itch
Automate claiming free itch.io games. The script scans sources like Reddit megathreads and itch.io sale pages for game URLs, processes them, then claims games any that are free. Thank you to u/Iron-Row for creating the original project

## Tl;dr

Run the script using the following arguments: ```python3 claim_itch.py --auto-login --skip-errors --ignore```. The `ignore` and `skip-errors` arguments should address *most* if-not all of the error conditions that the script runs into when trying to claim the games. The `auto-login` argument is optional and automatically logs into your itch.io account using credentials entered into login.cfg to enable the script to run unattended.

## Installation

### Windows instructions

1. Install [python](https://www.python.org/downloads/).
2. Install [Firefox](https://www.mozilla.org/firefox/).
3. Download the script (claim_itch.py) into a folder of your choice.
4. Download [geckodriver](https://github.com/mozilla/geckodriver/releases) and put the .exe in the same folder as the script (or in your [PATH](https://www.howtogeek.com/118594/how-to-edit-your-system-path-for-easy-command-line-access/)).
5. Open the folder. Click "File" then open Powershell (or Command Prompt). This is where we will execute commands and run the script.
6. Install required packages by typing the command `python -m pip install beautifulsoup4 lxml requests selenium` and click enter to execute it.

### Mac instructions

1. Install [Firefox](https://www.mozilla.org/firefox/).
2. Download the script (claim_itch.py) into a folder of your choice.
3. Open Terminal
4. Install [Homebrew](https://brew.sh) if you haven't already done so `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)`
5. Install python using Homebrew `brew install python`
6. Install geckodriver using Homebrew `brew install geckodriver`
7. Install required packages by typing the command `python3 -m pip install beautifulsoup4 lxml requests selenium` and click enter to execute it.

### Ubuntu Server instructions (i.e., via SSH)

1. connect to your ubuntu server via SSH
2. Install firefox `sudo apt install firefox`
3. Install python3 and pip3
4. Install dependencies `python3 -m pip install beautifulsoup4 lxml requests selenium`
5. Install gecko driver `wget https://github.com/mozilla/geckodriver/releases/download/v0.31.0/geckodriver-v0.31.0-linux64.tar.gz`, then extract it with `tar -xvzf https://github.com/mozilla/geckodriver/releases/download/v0.31.0/geckodriver-v0.31.0-linux64.tar.gz`, then make it executable `chmod +x geckodriver`, finally move it `sudo mv geckodrvier /usr/local/bin/`
6. Install xvfb to make Firefox run "headless" `sudo apt-get install xvfb`, then create a fake display using `sudo Xvfb :5 -ac`, and `export DISPLAY=:5`
7. Run the script using the automatic login argument `--auto-login` after setting it up in using the instructions in the next step below (mandatory, because you won't receive a Firefox pop-up to login).
8. (Optional) install screen to be able to keep the script running in the background after loggin out of your ssh session. `sudo apt install screen`, then use `screen` to create a new screen, run the script here, then use `control+a`, then `d` to detach the screen and keep it running. You can check back in later by using the command `screen -ls` to see the address of any screens you have running and connect to them with `screen -r [screen number from -ls]`

### (Optional) Automate itch.io login

1. Edit sample_login.cfg and enter your information, then remove "sample_" from the filename. Alternatively, create a new login.cfg file using a basic text-editor and save it to the same folder as your script with the following information:

```
[DEFAULT]

username = replace_this_text_with_your_itch.io_username
password = replace_this_text_with_your_itch.io_password
```

## Usage

### (Optional) add or remove sources

-  The claim_itch.py script currently supports reddit threads and itch.io sales/collections
- Open the script in Notepad++ or your usual script editor and add your URL to the `SOURCES` variable. 
- **Note:** Additional links are included in the script, but disabled by default due to the length of time it takes to scan and process them. You can review and enable any of these URLs by removing the leading `#`. Conversely, disable any old or stale links by adding `#` to the start of their URL.

### Running the script

1. After downloading or cloning the repo, and navigate to the claim_itch folder.
2. Run the script using the command `python claim_itch.py`. If you are using MacOS Monterey, you may need to run `python3 claim_itch.py` instead
   1. **Note:** To use the automatic-login function, add the arg `--auto-login`, e.g., `python3 claim_itch.py --auto-login`
   2. **Note:** Add the `--skip-errors` and `--ignore` arguments if you don't want the script to stop/interrupt when it encounters errors attempting to process non-standard game pages (e.g., for Patreon patrons only).

3. The script will print its progress on the screen. Firefox will open and run in the background
4. After it collects game links. It will open firefox and go to itch.io.
5. If you haven't set up auto-login, manually log in now, then click enter in the script window.
6. It'll print its progress as it claims games. Then print a summary of the results.

### Optional - review the results in the history.json file

- After the script has completed, you can manually review how each game URL was processed using the claim_itch.history.json file.

- Games are generally filed as:

  -     'claimed'           successfully claimed to your account
        'dl_only'           cannot be claimed, must be downloaded manually
        'web'               cannot be claimed or downloaded, web-only game
        'buy'               not for sale
        'claimed has_more'  success, and indicates that the game is connected to another sale
        'removed'           game does not exist
        'always_free'       dl_only game that is always free
        'error'							the script enountered an error processing that game

## Troubleshooting

#### How to stop the script while it's running?

- Click Ctrl+C.


#### How to check if new games where posted?

- Use the argument `--recheck` to recheck if new games were posted in your source URLs. 

- Use the argument `--recheck-groups` to rechack itch.io sales/collections 

#### How to see non-claimable games that I need to download?

- Use the argument `--show-history`


#### Why can't I see captcha images?

- I disabled images by default to save bandwidth. Use the `--enable-images` argument, or use the audio captcha by clicking the [headphone icon](https://lh3.googleusercontent.com/K3-D1VX2E3fWD4rHRoqqmogPU-a_SV48lDideMH3bKSGNUE0Z-UMP0R0HGlAL2I=w305-h458). 


#### It stopped before claiming all games

- The code cannot handle all kinds of games on itch.io, such as games that are for Patreon patrons only, or link to mobile apps. You can use the `--ignore`, and `--skip-errors` options to continue claiming games despite an error. If there is any output that could be useful to fix these bug or handle these pages, please create a pull request or start an issue

#### How to see all the script options?

- Use the argument `--help`


#### What files are created by the script?

* The history is stored in a file of your choice or a default file where you run the script (see `python claim_itch.py --help`).
* A log is stored in a default location where you run the script.
* Another log is left by geckodriver.

#### Is the script safe?

- It doesn't do anything shady, but it can. The code has access to your computer, files, the internet, and controls a browser where you'll enter your itch password. 
- Your login details are entered in plain-text in login.cfg and are used to log into your itch.io account when running the script. Your login credentials are not saved or shared anywhere else, but anyone who has access to your computer could view that file.
