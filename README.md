# Домашка 14 "Ansible"

vagrant поднимает две машины: 
    ansible - то где ставится ansible и запускаются плейбуки  client - то куда эти плейбуки играем и ставим nginx
последовательность действий:

    vagrant up
    vagrant ssh ansible
    ansible-playbook nginx.yml
    curl http://192.168.11.151:8080

