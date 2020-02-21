# STATISTIQUE

Souvent, on envie de recuperarer les données des visiteurs de notre site, mais comment le faire exactement.
Avec DJANGO, on peut utiliser une API qui permet de le faire avec User-agent de django.
Voyons-voir!


## install

      pip install pyyaml ua-parser user-agents
      pip install django-user-agents
      
## using api

### Method 1

      import requests
      import json

      url = "https://ipapi.com/ip_api.php?ip={}" # api
      ip ='196.180.177.45'                       # ip test 

      req = requests.get(url.format(ip))
      if req.status_code:
          data = json.loads(req.text)
          print(data)
      
     # ------------------ Getting ip  -------------#
    def visitor_ip_address(request):

      x_forwarded_for = request.META.get('HTTP_X_FORWARDED_FOR')

      if x_forwarded_for:
          ip = x_forwarded_for.split(',')[0]
      else:
          ip = request.META.get('REMOTE_ADDR')
      return ip
      
 ### Method 2
 
       def save_visitor_infos(request):
          present_date = datetime.datetime.now()
          try:
              #----- get visitor ip -----#
              x_forwarded_for = request.META.get('HTTP_X_FORWARDED_FOR')
              if x_forwarded_for:
                  ip = x_forwarded_for.split(',')[0]
              else:
                  ip = request.META.get('REMOTE_ADDR')    
              #----- check if ip adress is valid -----#

              try:
                  socket.inet_aton(ip)
                  ip_valid = True
              except socket.error:
                  ip_valid = False
              #----- check if ip adress is valid -----#
              if ip_valid:


                  ref_date_1 = present_date - datetime.timedelta(days=1)
                  ref_date_2 = present_date - datetime.timedelta(days=2)
                 
                 visitor_infos_user_obj = Visitor_Infos_user.objects.filter(ip_address=ip, page_visited=request.path, event_date__gte=ref_date_1)

                  if visitor_infos_user_obj.count() == 0:
                      new_visitor_infos_user = Visitor_Infos_user(
                            ip_address = ip,
                            page_visited = request.path,
                            event_date = present_date
                      )          
                      new_visitor_infos_user.save()

                  if visitor_infos_user_obj.count() != 0:
                      visitor_infos_user_obj.event_date = present_date
                      visitor_infos_user_obj.save()
          except:
              pass

          context_nb_vistors = 0
          ref_date = present_date - datetime.timedelta(minutes=5) 
          context_nb_vistors = Visitor_Infos_user.objects.filter(event_date__gte=ref_date).values_list('ip_address', flat=True).distinct().count()

          return {"context_nb_vistors":context_nb_vistors}
   



Bon bref encore beaucoup de recherche a faire, bonne suite a vous!
