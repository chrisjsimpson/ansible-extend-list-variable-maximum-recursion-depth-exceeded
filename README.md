# Ansible: maximum recursion depth exceeded

## (Help! I want to extend an existing variable (list) in my `group_vars`) I've tried many things, what can I do?

I've tried to combine them with "`+`" (e.g. "`my_list: "{{ my_list + additonal_items }}"`) in my `group_vars` file, but when I do I get "`maximum recursion depth exceeded`". I'm not able/willing to restructure or lean on [Ansible variable precedence](https://gist.github.com/ekreutz/301c3d38a50abbaad38e638d8361a89e), what can I do?

## Using `set_fact` - read on- I know you may think `set_fact` may not be what you want

We can add a task which conditionally runs, and expands the list as you wish:

```
  - name: Combine lists (because env == "LOCAL")
    ansible.builtin.set_fact:
      my_list: "{{ my_list + additonal_items }}"
    when: env == "LOCAL"
```

- Take a look at [./playbooks/group_vars/all.yml](./playbooks/group_vars/all.yml)

- Take a look at [./playbooks/main.yml](./playbooks/main.yml)

## Steps to reproduce
```
python3 -m venv venv
. ./venv/bin/activate
pip install ansible
ansible-playbook playbooks/main.yml
```



You've probably already read:

- https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_variables.html
- etc

Thoughts. If you're hitting this, there's probably a deeper structural layout issue (perhaps [Ansible variable precedence](https://gist.github.com/ekreutz/301c3d38a50abbaad38e638d8361a89e)) will help and / or a good old refactor. As the [poem](https://peps.python.org/pep-0020/) goes, "If the implementation is hard to explain, it's a bad idea. If the implementation is easy to explain, it may be a good idea." 