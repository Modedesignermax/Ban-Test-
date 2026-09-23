#!/usr/bin/env python3
"""
CyberSecurity Toolkit
Created by Max

Funktionen:
- Passwort-Stärke prüfen
- Sichere Passwörter generieren
- SHA-256 Hash einer Datei erstellen
- Datei-Integrität überprüfen
"""

import hashlib
import secrets
import string
from pathlib import Path


def check_password(password):
    score = 0

    if len(password) >= 12:
        score += 1
    if any(c.isupper() for c in password):
        score += 1
    if any(c.islower() for c in password):
        score += 1
    if any(c.isdigit() for c in password):
        score += 1
    if any(c in string.punctuation for c in password):
        score += 1

    levels = {
        0: "Sehr schwach",
        1: "Schwach",
        2: "Mittel",
        3: "Gut",
        4: "Stark",
        5: "Sehr stark"
    }

    return levels[score]


def generate_password(length=20):
    characters = string.ascii_letters + string.digits + string.punctuation
    return "".join(secrets.choice(characters) for _ in range(length))


def file_hash(filename):
    sha256 = hashlib.sha256()

    with open(filename, "rb") as file:
        while chunk := file.read(8192):
            sha256.update(chunk)

    return sha256.hexdigest()


def main():
    print("=" * 45)
    print("        CYBERSECURITY TOOLKIT")
    print("             by Max")
    print("=" * 45)

    while True:
        print("\n[1] Passwort prüfen")
        print("[2] Sicheres Passwort generieren")
        print("[3] SHA-256 Hash einer Datei")
        print("[4] Programm beenden")

        choice = input("\nAuswahl: ")

        if choice == "1":
            password = input("Passwort eingeben: ")
            print("Stärke:", check_password(password))

        elif choice == "2":
            try:
                length = int(input("Passwortlänge: "))
                print("\nGeneriertes Passwort:")
                print(generate_password(length))
            except ValueError:
                print("Bitte eine Zahl eingeben.")

        elif choice == "3":
            filename = input("Dateipfad: ")
            path = Path(filename)

            if path.is_file():
                print("\nSHA-256:")
                print(file_hash(filename))
            else:
                print("Datei wurde nicht gefunden.")

        elif choice == "4":
            print("Programm beendet.")
            break

        else:
            print("Ungültige Auswahl.")


if __name__ == "__main__":
    main()
