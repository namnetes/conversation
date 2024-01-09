#!/usr/bin/env python
# -*- coding: utf-8 -*-

from urllib.parse import urlparse
import re

class ProxyManager:
    """
    ProxyManager permet de valider et extraire des informations d'une ligne de proxy.
    """

    def __init__(self):
        pass

    def get_proxy_info(self, proxy_line):
        """
        Valide et extrait les informations du proxy Ã  partir de la ligne spÃ©cifiÃ©e.

        :param proxy_line: Ligne de proxy Ã  valider et extraire.
        :return: Dictionnaire contenant les informations du proxy ou None en cas d'erreur.
        """
        validation_pattern = re.compile(r'^\s*proxy\s*=\s*(?:(\w+):(\w+)@)?(https?://[a-zA-Z0-9.-]+):(\d+)\s*$', re.IGNORECASE)
        extraction_pattern = re.compile(r'^\s*proxy\s*=\s*(?:(\w+):(\w+)@)?(https?://[a-zA-Z0-9.-]+):(\d+)\s*$', re.IGNORECASE)

        validation_match = validation_pattern.match(proxy_line)
        if validation_match:
            proxy_line = validation_match.group(0)
            extraction_match = extraction_pattern.match(proxy_line)

            if extraction_match:
                proxy_dict = {
                    "Username": extraction_match.group(1),
                    "Password": extraction_match.group(2),
                    "URL": extraction_match.group(3),
                    "Port": extraction_match.group(4)
                }
                return proxy_dict
            else:
                raise ValueError("Erreur lors de l'extraction. La ligne aprÃ¨s validation ne correspond pas Ã  l'expression d'extraction.")
        else:
            raise ValueError("La ligne proxy n'est pas valide.")

    def get_hostname(self, url):
        """
        Extrait le nom d'hÃ´te Ã  partir de l'URL spÃ©cifiÃ©e.

        :param url: URL Ã  partir de laquelle extraire le nom d'hÃ´te.
        :return: Nom d'hÃ´te extrait.
        """
        parsed_url = urlparse(url)
        return parsed_url.netloc

# Exemple d'utilisation
proxy_manager = ProxyManager()
proxy_line = "proxy=alan:MotDePasse@https://localhost:3129"
try:
    proxy_info = proxy_manager.get_proxy_info(proxy_line)
    if proxy_info:
        print("Informations du proxy :", proxy_info)
        hostname = proxy_manager.get_hostname(proxy_info['URL'])
        print("Nom d'hÃ´te extrait :", hostname)
except ValueError as e:
    print(f"Erreur : {e}")