# STATISTIQUE

Souvent, on envie de recuperarer les données des visiteurs de notre site, mais comment le faire exactement.
Avec DJANGO, on peut utiliser une API qui permet de le faire avec User-agent de django.
Voyons-voir!


## INSTALLATION

      pip install pyyaml ua-parser user-agents
      pip install django-user-agents
      
## Utilisation de l'api

      import requests
      import json

      url = "https://ipapi.com/ip_api.php?ip={}" # api
      ip ='196.180.177.45'                       # ip test 

      req = requests.get(url.format(ip))
      if req.status_code:
          data = json.loads(req.text)
          print(data)
   
## Fonction de recuperation de l' IP

    def visitor_ip_address(request):

      x_forwarded_for = request.META.get('HTTP_X_FORWARDED_FOR')

      if x_forwarded_for:
          ip = x_forwarded_for.split(',')[0]
      else:
          ip = request.META.get('REMOTE_ADDR')
      return ip


Bon bref encore beaucoup de recherche a faire, bonne suite a vous!
