# mongodb

## Atividade Prática – MongoDB

### Exercício 1- Aquecendo com os pets

1. Adicione outro Peixe e um Hamster com nome Frodo:
<pre>&gt; db.pets.insert({name:&quot;Nemo&quot;,species:&quot;Peixe&quot;})
WriteResult({ &quot;nInserted&quot; : 1 })
</pre>
<pre>&gt; db.pets.insert({name:&quot;Frodo&quot;,species:&quot;Hamster&quot;})
WriteResult({ &quot;nInserted&quot; : 1 })
</pre>
2. Faça uma contagem dos pets na coleção
<pre>&gt; db.pets.find().count();
8
</pre>
3. Retorne apenas um elemento o método prático possível
<pre>&gt; db.pets.findOne()
{
	&quot;_id&quot; : ObjectId(&quot;5e95fc0dd2021919b3d6e6f4&quot;),
	&quot;name&quot; : &quot;Mike&quot;,
	&quot;species&quot; : &quot;Hamster&quot;
}
</pre>
4. Identifique o ID para o Gato Kilha.
<pre>&gt; db.pets.find({&quot;name&quot; : &quot;Kilha&quot;}, {&quot;_id&quot;:1});
{ &quot;_id&quot; : ObjectId(&quot;5e95fc0dd2021919b3d6e6f6&quot;) }
</pre>
5. Faça uma busca pelo ID e traga o Hamster Mike
<pre>&gt; db.pets.find({&quot;name&quot; : &quot;Mike&quot;, species:&quot;Hamster&quot;}, {&quot;_id&quot;:1});
{ &quot;_id&quot; : ObjectId(&quot;5e95fc0dd2021919b3d6e6f4&quot;) }
&gt; db.pets.find({&quot;_id&quot; :ObjectId(&quot;5e95fc0dd2021919b3d6e6f4&quot;)});
{ &quot;_id&quot; : ObjectId(&quot;5e95fc0dd2021919b3d6e6f4&quot;), &quot;name&quot; : &quot;Mike&quot;, &quot;species&quot; : &quot;Hamster&quot; }
</pre>
6. Use o find para trazer todos os Hamsters
<pre>&gt; db.pets.find({species:&quot;Hamster&quot;});
{ &quot;_id&quot; : ObjectId(&quot;5e95fc0dd2021919b3d6e6f4&quot;), &quot;name&quot; : &quot;Mike&quot;, &quot;species&quot; : &quot;Hamster&quot; }
{ &quot;_id&quot; : ObjectId(&quot;5e95fdc6cbecfa876c2045ba&quot;), &quot;name&quot; : &quot;Frodo&quot;, &quot;species&quot; : &quot;Hamster&quot; }
</pre>
7. Use o find para listar todos os pets com nome Mike
<pre>&gt; db.pets.find({&quot;name&quot; : &quot;Mike&quot;});
{ &quot;_id&quot; : ObjectId(&quot;5e95fc0dd2021919b3d6e6f4&quot;), &quot;name&quot; : &quot;Mike&quot;, &quot;species&quot; : &quot;Hamster&quot; }
{ &quot;_id&quot; : ObjectId(&quot;5e95fc0dd2021919b3d6e6f7&quot;), &quot;name&quot; : &quot;Mike&quot;, &quot;species&quot; : &quot;Cachorro&quot; }
</pre>
8. Liste apenas o documento que é um Cachorro chamado Mike
<pre>&gt; db.pets.find({&quot;name&quot; : &quot;Mike&quot;, species:&quot;Cachorro&quot;});
{ &quot;_id&quot; : ObjectId(&quot;5e95fc0dd2021919b3d6e6f7&quot;), &quot;name&quot; : &quot;Mike&quot;, &quot;species&quot; : &quot;Cachorro&quot; }
</pre>

### Exercício 2 – Mama mia!

1. Liste/Conte todas as pessoas que tem exatamente 99 anos. Você pode usar um count para indicar a quantidade.
<pre>&gt; db.italians.find({&quot;age&quot; : 99}).count()
0
</pre>
2. Identifique quantas pessoas são elegíveis atendimento prioritário (pessoas com mais de 65 anos)
<pre>&gt; db.italians.find({&quot;age&quot; : {&quot;$gt&quot;:65}}).count()
1826
</pre>
3. Identifique todos os jovens (pessoas entre 12 a 18 anos).
<pre>&gt; db.italians.find({&quot;age&quot; : {&quot;$gte&quot; : 12, &quot;$lte&quot; : 18}}).count()
854
</pre>
4. Identifique quantas pessoas tem gatos, quantas tem cachorro e quantas não tem nenhum dos dois
<pre>&gt; db.italians.find({cat: {$exists: true}}).count()
6003
&gt; db.italians.find({dog: {$exists: true}}).count()
3993
&gt; db.italians.find({$and:[{dog: {$exists: false}},{cat: {$exists: false}}]}).count()
2389
</pre>
5. Liste/Conte todas as pessoas acima de 60 anos que tenham gato
<pre>&gt; db.italians.find({$and:[{cat: {$exists: true}},{&quot;age&quot; : {&quot;$gt&quot;:60}}]}).count()
1457
</pre>
6. Liste/Conte todos os jovens com cachorro
<pre>&gt; db.italians.find({$and:[{dog: {$exists: true}},{&quot;age&quot; : {&quot;$gte&quot; : 12, &quot;$lte&quot; : 18}}]}).count()
344
</pre>
7. Utilizando o $where, liste todas as pessoas que tem gato e cachorro
<pre>&gt; db.italians.find({ $where: function () { 
...     if (this.cat!=null &amp;&amp; this.dog!=null){ 
...         return true; 
...     } else { 
...         return false;
...     }  
... }}).count()
2385
&gt; db.italians.find({$and:[{dog: {$exists: true}},{cat: {$exists: true}}]}).count()
2385
</pre>
8. Liste todas as pessoas mais novas que seus respectivos gatos.
<pre>&gt; db.italians.find({$and:[{cat:{$exists:true}},{$where:&quot;this.age&lt;this.cat.age&quot;}]}).count()
639
</pre>
9. Liste as pessoas que tem o mesmo nome que seu bichano (gatou ou cachorro)
<pre>&gt; db.italians.find({ $where: function () { 
...     var ret = false;
...     
...     if (this.cat!=null &amp;&amp; this.firstname==this.cat.name){ 
...         ret = true;
...     } 
... 
...     if (this.dog!=null &amp;&amp; this.firstname==this.dog.name){ 
...         ret = true;
...     }
... 
...     return ret;
... }}).count()
114
</pre>
10. Projete apenas o nome e sobrenome das pessoas com tipo de sangue de fator RH negativo
11. Projete apenas os animais dos italianos. Devem ser listados os animais com nome e idade. Não mostre o identificado do mongo (ObjectId)
12. Quais são as 5 pessoas mais velhas com sobrenome Rossi?
13. Crie um italiano que tenha um leão como animal de estimação. Associe um nome e idade ao bichano
14. Infelizmente o Leão comeu o italiano. Remova essa pessoa usando o Id.
15. Passou um ano. Atualize a idade de todos os italianos e dos bichanos em 1.
16. O Corona Vírus chegou na Itália e misteriosamente atingiu pessoas somente com gatos e de 66 anos. Remova esses italianos.
17. Utilizando o framework agregate, liste apenas as pessoas com nomes iguais a sua respectiva mãe e que tenha gato ou cachorro.
18. Utilizando aggregate framework, faça uma lista de nomes única de nomes. Faça isso usando apenas o primeiro nome
19. Agora faça a mesma lista do item acima, considerando nome completo.
20. Procure pessoas que gosta de Banana ou Maçã, tenham cachorro ou gato, mais de 20 e menos de 60 anos.

