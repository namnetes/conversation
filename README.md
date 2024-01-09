from urllib.parse import urlparse

def extract_right_part(url):
    parsed_url = urlparse(url)
    return parsed_url.netloc

# Exemple d'utilisation
url = "http://www.example.com:8080/path/to/page"
right_part = extract_right_part(url)

if right_part:
    print(f"La partie de droite de l'URL est : {right_part}")
else:
    print("Aucune partie de droite trouvÃ©e dans l'URL.")


import socket

def test_service(ip, port):
    try:
        # CrÃ©e une socket TCP
        s = socket.create_connection((ip, port), timeout=5)
        # Ferme la connexion
        s.close()
        return True
    except (socket.timeout, ConnectionRefusedError):
        return False

# Exemple d'utilisation
ip = "example.com"
port = 80

if test_service(ip, port):
    print(f"Le service est accessible Ã  l'adresse {ip}:{port}")
else:
    print(f"Le service n'est pas accessible Ã  l'adresse {ip}:{port}")

