---
nav_title: "Expressions régulières"
article_title: Expressions régulières
page_order: 5
description: "Cet article de référence couvre ce que sont les expressions régulières, comment commencer à les utiliser, et offre des fonctionnalités de débogueur pour valider et tester les expressions régulières."
page_type: Référence
tool:
  - Outils de test
---

# Expressions régulières avec Braze

{% include video.html id="3h5Xbhl-TxE" align="right" %}

> Cet article de référence couvre ce que sont les expressions régulières, comment commencer à les utiliser, et offre des fonctionnalités de débogueur pour valider et tester les expressions régulières.

Une expression régulière, communément appelée regex, est une séquence de caractères qui définit un motif de recherche. Les expressions régulières vous permettent de valider les regroupements de texte et d'effectuer des recherches et des remplacements d'actions. Chez Braze, nous mettons à profit les expressions régulières pour vous offrir une solution de correspondance plus souple dans votre segmentation et votre filtrage de campagne pour votre public cible.

Dans la vidéo fournie, nous vous montrons comment les expressions régulières peuvent être utilisées et testées sur le site [Regex101][regex]. Ci-dessous, nous offrons également un testeur d'expressions régulières, une feuille de triche utile, des exemples de données référencées dans la vidéo regex LAB, ainsi que des questions fréquemment posées.

## Ressource

- [Expression régulière](https://lab.braze.com/regular-expression-basics-for-braze) cours LAB
- <i class="fas fa-file-pdf"></i> \[Feuille de triche PDF\]\[cheatsheet\]
- <i class="fas fa-file-alt"></i> \[Exemple de données RTF\]\[dummydata\]

## Débogueur Regex

{% tabs %}
{% tab Regex Debugger %}

Ce formulaire permet la validation et le test de base des expressions régulières. ​
<div class="alert alert-important" role="alert"><div class="alert-msg"> <b>importance: </b><br />
<p>Cet outil est uniquement conçu comme une référence, et ne garantit pas que le regex corresponde à 100% avec la plateforme Braze. Expressions régulières dans Braze ajoute automatiquement le modificateur <code>gi</code>. Le modificateur <a href='https://w3schools.sinsixx.com/jsref/jsref_regexp_modifier_gi.asp.htm'>gi</a> est utilisé pour effectuer une recherche insensible à la casse de toutes les occurrences d'une expression régulière dans une chaîne. </p>
</div></div>
<div>
Regex:
 <unk>
<div class="input-group">
  <div class="input-group-prepend"><span class="input-group-text">/</span>
  </div>
 <input id="regex_input" value="" class="form-control" placeholder="regex" style="" />
 <div class="input-group-append"><span class="input-group-text">/gi</span>
 </div>
</div>
<br />
Valeur(s) de contrôle : <textarea style="" placeholder="chaîne de correspondance" id="regex_text"></textarea><br /><br />
<unk>
Résultats correspondants<span id="reg_count"></span>: <div id="regex_results"></div>
</div>
<style type="text/css">
#regex_text {
  -moz-appearance: textfield-multiline;
  -webkit-appearance: textarea;
  border: 1px solid #ced4da !important;
  overflow: auto;
  padding: 2px;
  resize: both;
  white-space: pre-wrap;
  width:100%;
  height: 250px;
  padding: 5px 15px 5px 1.2em;
  border-radius: 0.25rem;
}
#regex_input {
  border: 1px solid #ced4da !important;
  padding: 0 15px 0 5px;
}
#regex_input.invalid {
  background-color: #f8eef7;
}
.regex_highlight {
  background-color: #66d4b333;
}
#regex_results {
  width: 100%;
  min-height: 2em;
  padding: 5px 15px 5px 0.2em;
}
</style>
<script type="text/javascript">
$( document ).ready(function() {
  function update_inputmatch() {
    var tomatch = $('#regex_input').val();
    var validreg = true;
    $('#regex_input').removeClass('invalid');
    try {
      var regex = new RegExp(tomatch,'gi');
      $('#regex_results').html('');
    } catch(e) {
      $('#regex_input').addClass('invalid');
      validreg = false;
      $('#regex_results').html('Invalid Regular Expression').prepend('&nbsp;&nbsp;&nbsp;');
    }
    if (validreg){
      if ($('#regex_text').val() ) {
        if (tomatch) {
          var input_str = $('#regex_text').val().split(/\r?\n/);
          var input_replaced = [];
          var reg_count = 0;
          for (var i = 0; i < input_str.length; i++) {
            var inp_rep = ''
            var matched = input_str[i].match(regex);
            if (matched) {
              inp_rep = '<i class="far fa-check-square"></i> ';
              reg_count++;
            }
            else {
              inp_rep = '<i class="far fa-square"></i> ';
            }
            inp_rep += input_str[i].replace(regex,'<span class="regex_highlight">$&</span>');
            input_replaced.push(inp_rep)
          }
          if (reg_count) {
            $('#reg_count').html(' (' + reg_count + ')');
          }
          else {
            $('#reg_count').html('');
          }
          $('#regex_results').html(input_replaced.join('<br />'));
        }
      }
      else {
        $('#regex_results').html('');
      }
    }
  }
  $('#regex_input, #regex_text').keyup(function(k){
    update_inputmatch();
  });
});
</script>

{% endtab %}
{% endtabs %}

## Foire aux questions

#### Comment puis-je filtrer les adresses e-mail spécifiques à la boîte de réception lors de la segmentation ?

{% raw %}
Utilisez le filtre d'adresse e-mail, définissez-le à `correspondances regex`. Faites ensuite référence à la regex pour les adresses e-mail:

```
[a-zA-Z0-9.+_-]+@[a-zA-Z0-9.-]+\.[a-zA-Z.-]+
```

Nous pouvons briser cette regex pour les trois parties suivantes :

- `[a-zA-Z0-9.+_-]+` est le début de l'adresse e-mail avant le caractère `@`. Donc le "nom" dans "nom@exemple.com".
- `[a-zA-Z0-9.-]+` est la première partie du domaine. Donc l'"exemple" dans "nom@exemple.com".
- `[a-zA-Z.-]+` est la dernière partie du domaine. Donc le "com" dans "name@example.com".

{% endraw %}

#### Comment filtrer les adresses e-mail associées à un domaine spécifique ?

Dites que vous voulez filtrer pour les e-mails se terminant par "@braze.com". Vous utiliseriez le filtre d'adresse e-mail, définissez-le à `correspondances regex`et entrez "@braze.com" dans le champ regex. Il en va de même pour tout autre domaine de messagerie.

![image1]({% image_buster /assets/img/regex/regeximg1.png %})

#### Comment puis-je utiliser les chaînes de nombre de filtre pour les valeurs de †x ou de <unk> x?

Si vous recherchez des valeurs supérieures ou égales à (<unk> ) x, utilisez les expressions régulières suivantes :

```
^([x-y]|\d{z<unk> )$
```

Où `x-y` est la plage de nombres (0-9) du premier chiffre, et `z` est le plus le nombre de chiffres de x. Par exemple, pour les valeurs supérieures ou égales à 50, la regex serait alors `^([5-9][0-9]|\d{3,})$`.

Si vous recherchez des valeurs inférieures ou égales à (<unk> ) x, utilisez les expressions régulières suivantes :

```
^([x-y]|[a-b])$
```

Où `x-y` est la plage de nombres (0-9) du premier chiffre, et `a-b` est la plage limite inférieure de x. Par exemple, pour des valeurs inférieures ou égales à 50, le regex serait alors `^([5-9][0-9]|[0-4][0-9])$`.

#### Comment filtrer les attributs personnalisés qui commencent par une chaîne spécifique ?

Utilisez le symbole caret (`^`) pour indiquer par quoi commence la chaîne de caractères puis entrez le nom de l'attribut personnalisé que vous voulez spécifier.

Par exemple, si vous essayez de cibler les utilisateurs qui vivent dans des villes qui commencent par "San", votre regex serait `^San \w`. Avec ce regex, vous cibliez avec succès des utilisateurs de villes comme San Francisco, San Diego, San Jose, et ainsi de suite.

![image2]({% image_buster /assets/img/regex/regeximg2.png %})

#### Comment filtrer les numéros de téléphone spécifiques?

Avant d'utiliser regex pour filtrer les numéros de téléphone, rappelez-vous que les numéros enregistrés pour les profils d'utilisateur doivent être dans [E. 64 format](https://en.wikipedia.org/wiki/E.164), comme spécifié dans [les numéros de téléphone de l'utilisateur]({{site.baseurl}}/user_guide/message_building_by_channel/sms/phone_numbers/user_phone_numbers/).

En supposant que vous recherchez des numéros de téléphone américains, utilisez le format regex `1? d\d\d\d\d\d\d\d\d\d\d\d`, où chaque répétition de `\d` est un chiffre que vous voulez préciser. Les trois premiers chiffres sont l'indicatif régional.

De même, le format des numéros de téléphone au Royaume-Uni est `^\+4\d\d\d\d\d\d\d\d\d\d\d\d`. Tout autre pays serait le code de pays respectif, suivi du nombre nécessaire de `\d` répétitions pour chaque chiffre restant. Donc, dans le cas de la Lituanie avec un code pays de "3", leur regex serait `^\+3\d\d\d\d\d\d\d\d\d\d`.

Par exemple, disons que vous voulez filtrer les utilisateurs par numéro de téléphone pour un indicatif régional spécifique, "718". Utilisez le filtre de numéro de téléphone, définissez-le à `correspond à regex`, et entrez les regex suivants :

```
^1?718\d\d\d\d\d\d\d
```

![image3]({% image_buster /assets/img/regex/regeximg3.png %})
[cheatsheet]: {% image_buster /assets/download_file/regex-cheatsheet.pdf %} [dummydata]: {% image_buster /assets/download_file/regex-dummy-data.rtf %}


[regex]: https://regex101.com/