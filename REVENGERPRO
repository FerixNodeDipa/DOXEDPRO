const readline = require('readline');
const chalk = require('chalk');
const Table = require('cli-table3');
const fs = require('fs');
const { exec } = require('child_process'); // Fitur buka URL

let player;
try {
    player = require('play-sound')(opts = {});
} catch (e) {
    player = null;
}

const rl = readline.createInterface({ input: process.stdin, output: process.stdout });
const RED = chalk.hex('#880000'); 
const GOLD = chalk.hex('#FFD700'); 
const DB_FILE = './victims_db.json';

// Fungsi Buka Web Otomatis
function openWeb(url) {
    const cmd = process.platform === 'win32' ? `start ${url}` : `termux-open-url ${url} || xdg-open ${url}`;
    exec(cmd);
}

function playMusic() {
    if (player && fs.existsSync('./music.mp3')) {
        player.play('./music.mp3', function(err){
            if (!err) playMusic(); 
        });
    }
}

playMusic();

let savedVictims = [];
if (fs.existsSync(DB_FILE)) {
    try {
        savedVictims = JSON.parse(fs.readFileSync(DB_FILE));
    } catch (e) {
        savedVictims = [];
    }
}

// --- ASCII ART ---
const ARCI_BIG = `
⢰⣦⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⢀⣼
⠀⢿⣷⣄⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⢀⣀⣤⠴⠶⠖⠒⠛⣿⣽⡟⠛⠓⠒⠶⠶⢤⣀⣀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⢀⣀⠴⣯⠏
⠀⠀⢿⣯⣿⠷⣶⣤⡴⣶⣶⣤⠤⠤⣤⣄⡀⠀⣀⡤⠶⠛⠉⠀⠀⠀⠀⠀⠀⠴⠿⣼⠿⠦⠀⠀⠀⠀⠀⠀⠉⠛⠲⢦⣄⣀⣤⣴⠒⣻⣿⣿⢻⣿⣿⣿⠋⣩⣴⠋⠀
⠀⠀⠀⠙⠿⣦⣼⣟⢷⣷⢹⣿⣌⣿⡟⢺⣿⠛⠻⣄⠀⠀⠀⠀⢀⣠⣤⠤⠖⠒⠒⠛⠒⠒⠒⠦⠤⣤⣀⠀⠀⢀⣤⠖⠛⢿⣇⠐⡾⣷⣿⡟⢚⣿⣷⣿⠶⠋⠁⠀⠀
⠀⠀⠀⠀⠀⠈⠙⠛⠛⠻⠾⢿⣾⣮⣗⣸⣿⣆⠄⠀⠙⣦⡖⠛⠉⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠈⢉⣷⡟⠀⡀⢨⣽⣿⣽⣶⢿⡿⠛⠛⠉⠁⠀⠀⠀⠀⠀⠀
⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⢀⣴⠛⠉⠙⠻⢿⣷⣶⡂⣸⡟⠓⣆⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⣠⠞⠛⣧⣄⣿⣾⣿⡋⠉⠀⠀⠙⢦⡀⠀⠀⠀⠀⠀⠀⠀⠀
⠀⠀⠀⠀⠀⠀⠀⠀⠀⣠⠟⠁⠀⠀⠀⣠⠾⣿⣿⣿⣿⡁⠀⠈⢳⣀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⢀⣴⠋⠀⠀⣿⣿⣿⣫⣶⠟⢦⡀⠀⠀⣀⠹⣆⠀⠀⠀⠀⠀⠀⠀
⠀⠀⠀⠀⠀⠀⠀⢀⡼⢿⡷⣾⠀⢀⡞⠁⠀⠹⡄⢻⣿⣿⡆⠀⠘⣿⣦⣤⣀⣀⠀⠀⣀⣀⣤⣶⣿⡯⠀⢀⣾⣿⡿⠋⢀⡞⠀⠀⠙⢆⣀⣿⣻⣯⣷⡀⠀⠀⠀⠀⠀
⠀⠀⠀⠀⠀⠀⢀⡾⠷⠛⢳⡞⣻⠋⠀⠀⠀⠀⢳⡀⢻⣿⣿⣦⣠⣿⡯⣷⡉⣽⠿⠿⠟⠉⣹⡯⣿⣷⣤⣾⣿⣿⠁⠀⣼⠃⠀⠀⠀⠈⠻⡍⠏⠁⠉⢷⡀⠀⠀⠀⠀
⠀⠀⠀⠀⠀⢀⡾⠁⠀⠀⠈⡱⠁⠀⠀⠀⠀⠀⠈⢷⠈⣿⣿⣿⠟⣿⣇⡝⣮⡈⠀⠀⠀⣴⡟⠀⢿⡟⢿⣻⣿⣇⠀⣰⠇⠀⠀⠀⠀⠀⠀⠙⡄⠀⠀⠀⢿⡀⠀⠀⠀
⠀⠀⠀⠀⠀⣼⠇⠀⠀⠀⡼⠃⠀⠀⠀⣀⣀⣠⣤⣼⠿⣿⢿⠃⣰⠋⠈⠁⢻⡙⢶⣴⠟⣹⠃⠀⠀⠱⡄⢹⣿⣟⠲⢿⠤⠤⣤⣤⣀⡀⠀⠀⠸⡆⠀⠀⠈⣧⠀⠀⠀
⠀⠀⠀⠀⢠⡏⠀⠀⠀⣰⣃⣤⠖⠋⣉⡡⢆⣠⠟⠁⣼⣿⡿⢸⣇⠶⠊⠀⢸⣷⠛⠉⢳⣿⠀⠀⠐⢶⠹⡌⣿⡿⣆⠈⠱⢦⡐⠦⣄⣉⣙⣶⣄⣹⡀⠀⠀⢸⡇⠀⠀
⠀⠀⠀⠀⣾⠀⠀⢰⣶⣿⣿⣿⡿⢿⣥⣶⣟⣁⣠⣞⣽⠟⡇⢸⣿⡀⣀⡴⠋⢹⡄⠀⣸⣉⣻⣦⣄⣸⣰⡇⣿⢹⣮⣷⣤⣤⣿⠿⠞⣛⣿⣿⣿⣿⡿⠂⠀⠀⢿⠀⠀
⠀⠀⠀⠀⡟⠀⠀⠀⢹⠋⠳⢿⣿⣷⡶⠦⢭⣽⣾⣿⡟⠰⢷⣘⣿⠁⠿⠋⠉⠙⣿⠉⡿⠉⠉⠉⠏⢩⣿⢠⣟⣐⣿⣿⢷⣾⣷⣒⣩⣿⠿⠟⠉⠀⢱⠀⠀⠀⢸⠀⠀
⠀⠀⠀⢠⡇⠀⠀⠀⢸⠀⠀⠀⠈⠙⠻⠿⢿⣿⣿⣿⡟⣠⣾⣳⣽⣷⣦⢠⠄⣖⢹⣿⠃⠃⠠⠂⣰⣿⣿⢿⣧⣄⣻⣿⣿⣛⠟⠛⠋⠀⠀⠀⠀⠀⢸⡄⠀⠀⢸⡇⠀
⠀⠀⠀⢸⡇⠀⠀⠀⢸⠀⠀⠀⠀⠀⠀⢀⣸⠿⠋⢻⣿⣿⣿⣿⡽⣽⣾⣿⣦⣬⠞⠏⠀⢤⣼⣿⣿⣿⢱⣿⣿⣿⣿⡆⠈⠙⠲⣤⡀⠀⠀⠀⠀⠀⢸⡇⠀⠀⢸⡇⠀
⠀⠀⠀⠀⡇⢰⣄⣤⣾⠀⠀⠀⢀⣠⠶⠋⠁⠀⢀⣾⣿⢿⣿⣾⣇⢹⡏⣻⣿⠞⠀⠀⠀⠰⣿⣏⣸⡇⣾⣿⣿⣿⣿⣿⠀⠀⠀⠀⠙⠳⢦⣀⠀⠀⢸⢳⣦⡞⢸⡇⠀
⠀⠀⠀⠀⣷⡼⣯⡽⢿⣆⣤⣞⣋⣀⣀⣀⣀⣀⣸⣿⣿⣧⠬⠹⣯⣬⣿⠉⠹⣄⠀⠀⠀⣰⠏⠉⣿⢤⣿⠟⠲⣾⣿⣻⣧⣤⣤⣤⡤⠤⠤⠽⠿⢦⡼⠛⣷⠛⢿⠀⠀
⠀⠀⠀⠀⢻⡄⠘⠃⠀⢿⠀⠀⠀⠀⠀⠀⠀⠀⠘⢿⣿⣷⣄⠀⢻⣿⠏⢦⠀⠈⠐⠀⠸⡁⠀⡟⠙⣿⠟⠀⣠⣾⣿⣾⠃⠀⠀⠀⠀⠀⠀⠀⠀⢠⡇⠀⠙⢀⡿⠀⠀
⠀⠀⠀⠀⠘⣇⠀⠀⠀⠈⡇⠀⠀⠀⠀⠀⠀⠀⠀⠈⢿⣿⣿⣷⣄⠿⣄⠈⢿⡆⠀⠀⠀⢴⡿⠀⣠⠟⣠⣾⣿⢿⡽⠁⠀⠀⠀⠀⠀⠀⠀⠀⠀⡞⠀⠀⠀⣸⠇⠀⠀
⠀⠀⠀⠀⠀⢹⡆⠀⠀⠀⠘⣆⠀⠀⠀⠀⠀⠀⠀⠀⠈⢿⣿⣿⢿⣶⡈⢦⢸⡇⠀⢠⠀⢸⡇⠐⢁⣼⣿⢿⣯⡟⠁⠀⠀⠀⠀⠀⠀⠀⠀⠀⡼⠁⠀⠀⢠⡏⠀⠀⠀
⠀⠀⠀⠀⠀⠀⢻⡄⠀⠀⠀⠘⣆⠀⠀⠀⠀⠀⠀⠀⠀⠀⠙⢶⣿⣿⣳⠀⠀⡇⠀⣼⠀⢸⡇⠀⣜⣿⣹⠶⠋⠀⠀⠀⠀⠀⠀⠀⠀⠀⢀⡼⠁⠀⠀⢀⡿⠀⠀⠀⠀
⠀⠀⠀⠀⠀⠀⠀⢻⡄⠀⠀⠀⠈⢣⡀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠈⠙⣯⡃⢸⡇⠀⢹⠂⠈⡇⠀⣿⠋⠁⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⢠⠞⠀⠀⠀⢠⡞⠁⠀⠀⠀⠀
⠀⠀⠀⠀⠀⠀⠀⠀⠹⣆⠀⠀⠀⠀⠙⢦⡀⠀⠀⠀⠀⠀⠀⠀⠀⠀⣽⠷⣼⠃⠀⠀⠀⠀⢷⡰⢹⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⢀⠔⠁⠀⠀⠀⣠⠟⠀⠀⠀⠀⠀⠀
⠀⠀⠀⠀⠀⠀⠀⠀⠀⠈⢷⣄⠀⠀⠀⠀⠙⢦⣀⠀⠀⠀⠀⠀⠀⠀⢿⣴⣿⣦⣀⣠⣀⣤⣿⣧⢾⠆⠀⠀⠀⠀⠀⠀⠀⣠⠶⠃⠀⠀⠀⢀⡼⠋⠀⠀⠀⠀⠀⠀⠀
⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠙⢦⡀⠀⠀⠀⣆⣉⣷⢦⣀⠀⠀⠀⠀⢸⡜⠿⣷⣿⣿⠿⣽⡿⠛⡞⠀⠀⠀⠀⢀⣠⣴⢊⣁⠀⠀⠀⢀⣴⠟⠀⠀⠀⠀⠀⠀⠀⠀⠀
⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠙⠦⣄⣠⢿⣩⡷⡄⠈⠙⠓⠤⢤⣀⣙⣦⣈⣻⣦⣾⣁⣠⣞⣁⣀⠤⠴⠚⠋⣀⣿⣻⣧⡀⣀⡴⠋⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀
⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠈⠑⠦⣟⡀⠀⠀⠀⠀⠀⠀⠀⠉⠉⢿⡿⠷⣿⢿⡯⠉⠉⠀⠀⠀⠀⠀⠉⠉⣿⡾⠛⠁⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀
⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠉⠓⠶⣄⣀⡀⠀⠀⠀⠀⠀⠙⣶⡿⢸⠇⠀⠀⠀⠀⣀⣠⠴⠞⠋⠁⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀
⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠉⠛⠛⠒⠒⠲⢾⣟⡥⠿⠒⠒⠛⠛⠉⠁⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀
           ___ _____   _____ _  _  ___ ___ ___     ___   _____  __
          | _ \\ __\\ \\ / / __| \\| |/ __| __| _ \\___|   \\ / _ \\ \\/ /
          |   / _| \\ V /| _|| .\` | (_ | _||   /___| |) | (_) >  < 
          |_|_\\___| \\_/ |___|_|\\_|\\___|___|_|_\\   |___/ \\___/_/\\_\\
`;

const ARCI_DOXXING = `
⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⢀⡀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⢠⡀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀
⠀⠀⠀⠀⠀⠀⠀⠀⠀⢀⠔⡿⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠹⡒⢄⠀⠀⠀⠀⠀⠀⠀⠀⠀
⠀⠀⠀⠀⠀⠀⠀⢀⡖⠁⣸⠃⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⣧⠈⢳⣄⠀⠀⠀⠀⠀⠀⠀
⠀⠀⠀⠀⠀⠀⢠⡟⠀⠀⣿⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⢻⡄⠀⢹⣆⠀⠀⠀⠀⠀⠀
⠀⠀⠀⠀⠀⢠⡿⠀⠀⢠⡗⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⢸⡇⠀⠀⢻⡆⠀⠀⠀⠀⠀
⠀⠀⠀⠀⠀⣾⠁⠀⠀⢸⣿⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⢸⡇⠀⠀⠈⣯⠀⠀⠀⠀⠀
⠀⠀⠀⠀⢰⡞⠀⠀⠀⠈⢻⡀⠀⠀⠀⠀⠀⣀⣤⡴⠶⠞⠛⠛⠛⠛⠛⠻⠶⢶⣤⣀⠀⠀⠀⠀⠀⠀⣿⠃⠀⠀⠀⢸⡇⠀⠀⠀⠀
⠀⠀⠀⠀⢸⡇⠀⠀⠀⠀⠘⣷⡀⠀⣀⡴⢛⡉⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠉⡛⢦⣄⠀⠀⣼⠇⠀⠀⠀⠀⢸⡇⠀⠀⠀⠀
⢰⡀⠀⠀⢸⡇⠀⠀⠀⠀⠀⠈⠳⣾⣭⢤⣄⠘⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠃⢀⡤⣈⣷⠞⠃⠀⠀⠀⠀⠀⢸⡇⠀⠀⠀⡄
⢸⢷⡀⠀⠈⣿⠀⠀⠀⠀⠀⠀⠀⠀⠈⢉⡏⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⢸⡍⠀⠀⠀⠀⠀⠀⠀⠀⠀⣼⠃⠀⠀⡜⡇
⠈⣇⠱⣄⠀⠸⣧⠀⠀⠀⠀⠀⠄⣀⣀⣼⠁⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⢷⣀⣀⠠⠀⠀⠀⠀⠀⣰⠇⠀⢀⠞⢰⠃
⠀⢿⠀⠈⢦⡀⠘⢷⣄⠀⢀⣀⡀⣀⡼⠃⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠈⢷⣀⣀⡀⢀⠀⣠⡼⠋⢀⡴⠁⠀⣹⠀
⠀⠸⡄⠑⡀⠉⠢⣀⣿⠛⠒⠛⠛⠋⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠉⠛⠛⠒⠋⢻⣀⠴⠋⢀⠄⢀⡇⠀
⠀⠀⢣⠀⠈⠲⢄⣸⡇⠀⠀⠀⠠⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⢔⠀⠀⠀⠘⣏⣀⠔⠁⠂⡸⠀⠀
⠀⠀⠘⡄⠀⠀⠀⠉⢻⡄⠀⠀⠁⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠃⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⢠⡾⠋⠀⠀⠀⢠⠇⠀⠀
⠀⠀⠀⠙⢶⠀⠀⠀⢀⡿⠀⠤⣄⣀⠀⠀⠀⠀⠀⠀⠀⠀⢠⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⢀⡠⠤⠀⢾⡀⠀⠀⠀⡴⠎⠀⠀⠀
⠀⠀⠀⠀⠀⠙⢦⡀⣸⠇⠀⠀⠀⠈⠹⡑⠲⠤⣀⡀⠀⠀⢸⠀⠀⠀⠀⠀⠀⠀⣀⡤⠖⢊⠍⠃⠀⠀⠀⠘⣧⢀⡤⠊⠀⠀⠀⠀⠀
⠀⠀⠀⠀⠀⠀⠀⠉⣿⠀⠀⠀⠀⠀⠀⠈⠒⢤⠤⠙⠗⠦⠼⠀⠀⠀⠠⠴⠺⠟⠤⡤⠔⠁⠀⠀⠀⠀⠀⠀⢸⠋⠀⠀⠀⠀⠀⠀⠀
⠀⠀⠀⠀⠀⠀⠀⠀⠻⣦⡀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⣠⠟⠀⠀⠀⠀⠀⠀⠀⠀
⠀⠀⠀⠀⠀⠀⠀⠀⠀⠈⢳⣄⠀⠀⡑⢯⡁⠀⠀⠀⠀⠀⠇⠀⠀⠀⠰⠀⠀⠀⠀⠀⢈⡩⢋⠀⠀⢠⡾⠁⠀⠀⠀⠀⠀⠀⠀⠀⠀
⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⢻⡆⠀⠈⠀⠻⢦⠀⠀⠀⡰⠀⠀⠀⠀⠀⢇⠀⠀⠀⡠⡛⠀⠁⠀⢰⡿⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀
⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠈⣇⠀⠀⠀⠀⢡⠑⠤⣀⠈⢢⠀⠀⠀⡴⠃⣀⠤⠊⡄⠀⠀⠀⠀⢸⠇⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀
⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠙⢶⣄⠀⠀⠀⠳⠀⢀⠉⠙⢳⠀⡜⠉⠁⡀⠀⠼⠀⠀⠀⣠⡴⠛⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀
⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠈⠻⣦⠀⠘⣆⠐⠐⠌⠂⠚⠀⠡⠊⠀⢠⠃⠀⣠⠞⠉⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀
⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠘⣧⠠⠈⠢⣄⡀⠀⠀⠀⢀⣀⠴⠃⠀⣴⠇⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀
⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠙⢦⡁⠐⠀⠈⠉⠁⠈⠁⠀⠒⢀⡴⠛⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀
⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠙⣦⠀⠀⠀⠀⠀⠀⠀⣰⠟⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀
⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠘⢧⣄⣀⣀⣀⣀⣼⠃⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀
⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠈⠉⠉⠉⠁⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀`;

function showHeader(useAlternativeArci = false) {
    process.stdout.write('\x1Bc'); 
    if (useAlternativeArci) {
        console.log(RED(ARCI_DOXXING)); 
    } else {
        console.log(RED(ARCI_BIG)); 
    }
    
    const table = new Table({
        colWidths: [useAlternativeArci ? 50 : 78], 
        style: { border: ['red'] }
    });
    
    table.push(
        [{ content: GOLD(`Creator : Ferix / Dipa`), hAlign: 'center' }],
        [{ content: GOLD(`Date : ${new Date().toLocaleString()}`), hAlign: 'center' }],
        [{ content: GOLD(`Target Upload : Doxbin.net / Doxbin.com`), hAlign: 'center' }],
        [{ content: RED(`Copyright © Ferix & Dipauu`), hAlign: 'center' }]
    );
    console.log(table.toString());
}

function login() {
    process.stdout.write('\x1Bc');
    rl.question(RED("ENTER PASSWORD: "), (pw) => {
        if (pw === "itaiyo") {
            // FITUR OTOMATIS 1: Buka Profil Doxbin setelah Login
            openWeb("https://doxbin.com/user/DipauJelexky");
            mainMenu();
        } else {
            console.log(RED("ACCESS DENIED."));
            process.exit();
        }
    });
}

function mainMenu() {
    showHeader(false);
    const m = new Table({
        head: [GOLD('ID'), GOLD('DIPAUU MENU LIST')],
        colWidths: [6, 70]
    });
    
    m.push(['1', 'UPDATE DOXXING (Input)'], ['2', 'SAVED DATA (Database)'], ['0', 'EXIT']);
    console.log(m.toString());
    
    rl.question(GOLD("\nSelect Option > "), (c) => {
        if (c === '1') update();
        else if (c === '2') saved();
        else process.exit();
    });
}

async function update() {
    showHeader(true); 
    console.log(GOLD("\n          --- DOX BY DIPAU - INPUT FORM ---"));
    
    const fields = [
        "FULL NAME", "AGE", "HEIGHT", "WEIGHT", "EYE COLOR", "SKIN COLOR",
        "SCHOOL OR UNIVERSITY", "WORKING SALARY", "WORKPLACE", "CITY", "COUNTRY",
        "GAME ACCOUNT", "TIKTOK", "DISCORD", "INSTAGRAM", "X / TWITTER", "FACE PHOTO",
        "FACEBOOK", "E-MAIL", "BANK", "PHONE NUMBER", "PROVIDER", "WIFI PASSWORD",
        "WIFI NAME", "IP ADDRESS", "COORDINATES", "HOBBY", "RELIGION", "GENDER"
    ];

    let data = {};
    for (let f of fields) {
        data[f] = await new Promise(res => rl.question(GOLD(`${f} : `), res));
    }

    rl.question(GOLD("\n dipauu_cmd (/save to store) > "), (cmd) => {
        if (cmd.toLowerCase() === "/save") {
            savedVictims.push(data);
            fs.writeFileSync(DB_FILE, JSON.stringify(savedVictims, null, 2));
            console.log(GOLD("[✓] DATA SAVED."));
            
            // FITUR OTOMATIS 2: Buka Web Doxbin setelah simpan form
            console.log(RED("[!] Opening Doxbin for upload..."));
            openWeb("https://doxbin.com/"); 
        }
        setTimeout(mainMenu, 2000);
    });
}

function saved() {
    showHeader(false);
    console.log(GOLD("\n          === VICTIMS DATABASE ==="));

    if (savedVictims.length === 0) {
        console.log(RED("\n[!] DATABASE IS EMPTY."));
    } else {
        savedVictims.forEach((v, i) => {
            const t = new Table({
                head: [{content: GOLD(`Victim Index ID: #${i+1}`), colSpan: 2}],
                colWidths: [25, 50]
            });
            for (let [k, val] of Object.entries(v)) {
                t.push([GOLD(k), val === "" ? RED("(Empty)") : val]);
            }
            console.log(t.toString());
            console.log("\n");
        });
    }

    rl.question(GOLD("\n database_cmd (Enter to return) > "), (cmd) => {
        const cleanCmd = cmd.trim().toLowerCase();
        if (cleanCmd === "/delete all") {
            savedVictims = [];
            fs.writeFileSync(DB_FILE, "[]");
            setTimeout(mainMenu, 1000);
        } else if (!isNaN(cleanCmd) && cleanCmd !== "") {
            const index = parseInt(cleanCmd) - 1;
            if (index >= 0 && index < savedVictims.length) {
                savedVictims.splice(index, 1);
                fs.writeFileSync(DB_FILE, JSON.stringify(savedVictims, null, 2));
                setTimeout(saved, 1000);
            }
        } else {
            mainMenu();
        }
    });
}

login();

