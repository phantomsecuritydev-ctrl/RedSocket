#!/usr/bin/env python3

import os
import shutil
import subprocess
import sys
import urllib.request
import re
import hashlib
import time
import sys

# URL del file su GitHub (l'ultima versione del file)
GITHUB_RAW_URL = "https://raw.githubusercontent.com/phantomsecuritydev-ctrl/RedSocket/refs/heads/main/RedSocket.py"

# Ottieni la directory corrente in cui si trova lo script
current_directory = os.path.dirname(os.path.abspath(__file__))
redsocket_file_path = os.path.join(current_directory, "RedSocket.py")

# Funzione di debug
def debug():
    print(f"Debugging Iniziale - Current Directory: {current_directory}")
    print(f"Debugging Iniziale - RedSocket File Path: {redsocket_file_path}")
    if os.path.exists(redsocket_file_path):
        print("Il file RedSocket.py esiste!")
    else:
        print("Il file RedSocket.py NON esiste!")

# Funzione per ottenere il contenuto del file e calcolare l'hash
def get_file_hash(file_path):
    """Calcola l'hash SHA256 del file per comparare le versioni"""
    sha256_hash = hashlib.sha256()
    try:
        with open(file_path, "rb") as f:
            # Legge il file in blocchi di 4K
            for byte_block in iter(lambda: f.read(4096), b""):
                sha256_hash.update(byte_block)
    except FileNotFoundError:
        print(f"File non trovato: {file_path}")
        return None
    return sha256_hash.hexdigest()

# Funzione per eseguire l'aggiornamento
def update_script():
    print("Updating RedSocket...")

    # Verifica se il percorso del file esiste
    if not os.path.exists(current_directory):
        print(f"Directory non trovata: {current_directory}")
        sys.exit(1)

    try:
        # Scarica il file da GitHub e ottieni il contenuto
        print(f"Scaricando l'ultima versione da: {GITHUB_RAW_URL}")
        response = urllib.request.urlopen(GITHUB_RAW_URL)
        latest_script = response.read()

        # Calcola l'hash della versione scaricata
        latest_hash = hashlib.sha256(latest_script).hexdigest()

        # Calcola l'hash del file locale
        local_hash = get_file_hash(redsocket_file_path)

        if local_hash is None:
            print(f"Impossibile trovare il file {redsocket_file_path}. Verifica che il file esista.")
            sys.exit(1)

        if latest_hash == local_hash:
            print("Hai già l'ultima versione di RedSocket!")
        else:
            # Salva la versione aggiornata nel file
            with open(redsocket_file_path, "wb") as f:
                f.write(latest_script)
            print(f"Update complete! The script has been updated at {redsocket_file_path}")

            # Riavvia lo script aggiornato
            print("Riavvio dello script...")
            time.sleep(2)  # Aspetta 2 secondi prima di riavviare
            os.execv(sys.executable, ['python'] + sys.argv)  # Riavvia il programma

    except urllib.error.URLError as e:
        print(f"Update failed: Network issue or invalid URL. {e}")
    except PermissionError as e:
        print(f"Update failed: Permission issue. {e}")
    except Exception as e:
        print(f"Update failed: Unexpected error. {e}")

    # Torna al menu principale
    main_menu()  # Ritorna al menu principale dopo l'aggiornamento

# Funzione per mostrare l'aiuto
def show_help():
    print("""
    RedSocket - Help

    Comandi disponibili:
    --update    : Esegui l'aggiornamento dello script.
    --help      : Mostra questo messaggio di aiuto.
    scan        : Avvia una scansione di rete.
    """)

    # Torna al menu principale
    input("Premi Enter per tornare al menu principale...")  # Aspetta l'input dell'utente per tornare al menu
    main_menu()  # Ritorna al menu principale dopo aver mostrato l'aiuto

# Funzione per eseguire la scansione
def run_scan():
    # ASCII art di benvenuto
    print("\33[31m")
    ascii_art = '''
       (`-')  (`-')  _ _(`-')    (`-').->                    <-.(`-')  (`-')  _(`-')
    <-.(OO )  ( OO).-/( (OO ).-> ( OO)_      .->    _         __( OO)  ( OO).-/( OO).->
    ,------,)(,------. \\    .'_ (_)--\\_)(`-')----.  \\-,-----.'-'. ,--.(,------./    '._
    |   /`. ' |  .---' '`'-..__)/    _ /( OO).-.  '  |  .--./|  .'   / |  .---'|'--...__)
    |  |_.' |(|  '--.  |  |  ' |\\_..`--.( _) | |  | /_) (`-')|      /)(|  '--. `--.  .--'
    |  .   .' |  .--'  |  |  / :.-._)  \\ \\|  |)|  | ||  |OO )|  .   '  |  .--'    |  |
    |  |\\  \\  |  `---. |  '-'  /\\       / '  '-'  '(_'  '--'\\|  |\\   \\ |  `---.   |  |
    `--' '--' `------' `------'  `-----'   `-----'    `-----'`--' '--' `------'   `--'
                   REDSOCKET V.1
    '''
    print(ascii_art)

    # Controllo se Nmap è installato
    if shutil.which("nmap") is None:
        print("\33[31m Nmap is not installed")
        exit(1)

    # Input target
    target = input("\33[31m Insert target (IP OR DOMAIN): ")

    # Nome file automatico
    file = f"redsocket_{target}.txt"

    # Menu scan
    print("\33[31m \nChoose scan type: ")
    print("\33[31m 1 - Quick scan (common ports)")
    print("\33[31m 2 - Full scan (all ports)")

    choice = input("Choice: ").strip()

    # Costruzione del comando
    if choice == "1":
        command = ["nmap", "-F", "-sV", "-oN", file, target]
    elif choice == "2":
        command = ["sudo", "nmap", "-sS", "-p-", "-sV", "-oN", file, target]
    else:
        print("\33[31m Invalid choice. Exiting.")
        exit(1)

    # Funzione per eseguire Nmap con la barra di progresso
    def run_nmap_with_progress(command):
        # Esegui il comando con subprocess e cattura l'output
        process = subprocess.Popen(command, stdout=subprocess.PIPE, stderr=subprocess.PIPE, text=True)

        # Inizializza la barra di progresso
        with tqdm(total=100, desc="Scanning", unit="%", ncols=100) as pbar:
            # Leggi l'output di Nmap riga per riga
            for line in process.stdout:
                print("Nmap output:", line.strip())  # Aggiungi il debug qui per capire l'output

                # Cerca la percentuale nell'output di Nmap (Nmap mostra qualcosa come "80% done")
                if "percent done" in line.lower():
                    match = re.search(r"(\d+)% done", line)
                    if match:
                        percent = int(match.group(1))
                        pbar.n = percent  # Imposta la percentuale nella barra
                        pbar.last_print_n = percent
                        pbar.update(0)

            # Aspetta che il processo finisca
            process.wait()

        return process

    # Avvio scansione con la barra di progresso
    print("\33[31m \nScanning in progress...\n")
    run_nmap_with_progress(command)

    # Fine
    print("\33[31m \nScan completed!")
    print(f"\33[31m Results saved in: {file}")

# Funzione per il menu iniziale
def main_menu():
    print("\33[31m")
    print("Benvenuto in RedSocket v1!")
    print("\nScegli cosa vuoi fare:")
    print("1 - Esegui aggiornamento (Update)")
    print("2 - Mostra aiuto (Help)")
    print("3 - Inizia scansione (Scan)")

    choice = input("\nScegli una opzione (1, 2, 3): ").strip()

    if choice == "1":
        update_script()  # Esegui l'aggiornamento
    elif choice == "2":
        show_help()  # Mostra l'aiuto
    elif choice == "3":
        run_scan()  # Avvia la scansione
    else:
        print("\33[31m Opzione non valida. Riprova.")
        main_menu()  # Chiedi nuovamente l'input

# Debug iniziale
debug()

# Avvia il menu principale all'inizio
main_menu()
