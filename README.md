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
&gt; db.italians.find({ $or: [{ $and: [ { dog: { $exists: true}}, { $where: &quot;this.firstname== this.dog.name&quot; } ] },{ $and: [ { cat: { $exists: true}}, { $where: &quot;this.firstname== this.cat.name&quot; } ] }]}).count()
114
</pre>
10. Projete apenas o nome e sobrenome das pessoas com tipo de sangue de fator RH negativo
<pre>&gt; db.italians.find({&quot;bloodType&quot; : /-/}, {&quot;firstname&quot;: 1, &quot;surname&quot;: 1, &quot;bloodType&quot; : 1, &quot;_id&quot;:0})
{ &quot;firstname&quot; : &quot;Manuela&quot;, &quot;surname&quot; : &quot;Battaglia&quot;, &quot;bloodType&quot; : &quot;B-&quot; }
{ &quot;firstname&quot; : &quot;Chiara&quot;, &quot;surname&quot; : &quot;Mazza&quot;, &quot;bloodType&quot; : &quot;A-&quot; }
{ &quot;firstname&quot; : &quot;Salvatore&quot;, &quot;surname&quot; : &quot;Gentile&quot;, &quot;bloodType&quot; : &quot;A-&quot; }
{ &quot;firstname&quot; : &quot;Angela&quot;, &quot;surname&quot; : &quot;Battaglia&quot;, &quot;bloodType&quot; : &quot;B-&quot; }
{ &quot;firstname&quot; : &quot;Enrico&quot;, &quot;surname&quot; : &quot;Pellegrino&quot;, &quot;bloodType&quot; : &quot;O-&quot; }
{ &quot;firstname&quot; : &quot;Ilaria&quot;, &quot;surname&quot; : &quot;Coppola&quot;, &quot;bloodType&quot; : &quot;B-&quot; }
{ &quot;firstname&quot; : &quot;Laura&quot;, &quot;surname&quot; : &quot;Messina&quot;, &quot;bloodType&quot; : &quot;B-&quot; }
{ &quot;firstname&quot; : &quot;Matteo&quot;, &quot;surname&quot; : &quot;Costa&quot;, &quot;bloodType&quot; : &quot;AB-&quot; }
{ &quot;firstname&quot; : &quot;Massimo&quot;, &quot;surname&quot; : &quot;Marini&quot;, &quot;bloodType&quot; : &quot;O-&quot; }
{ &quot;firstname&quot; : &quot;Maurizio&quot;, &quot;surname&quot; : &quot;Pellegrini&quot;, &quot;bloodType&quot; : &quot;B-&quot; }
{ &quot;firstname&quot; : &quot;Emanuela&quot;, &quot;surname&quot; : &quot;Leone&quot;, &quot;bloodType&quot; : &quot;O-&quot; }
Type &quot;it&quot; for more
</pre>
11. Projete apenas os animais dos italianos. Devem ser listados os animais com nome e idade. Não mostre o identificado do mongo (ObjectId)
<pre>&gt; db.italians.find({$or: [{dog:{$exists:true}},{cat:{$exists:true}}]  },{&quot;dog.name&quot;:1, &quot;dog.age&quot;: 1, &quot;cat.name&quot;:1, &quot;cat.age&quot; : 1, &quot;_id&quot; : 0})
{ &quot;cat&quot; : { &quot;name&quot; : &quot;Mirko&quot;, &quot;age&quot; : 7 }, &quot;dog&quot; : { &quot;name&quot; : &quot;Michele&quot;, &quot;age&quot; : 6 } }
{ &quot;cat&quot; : { &quot;name&quot; : &quot;Giovanni&quot;, &quot;age&quot; : 13 }, &quot;dog&quot; : { &quot;name&quot; : &quot;Gianluca&quot;, &quot;age&quot; : 0 } }
{ &quot;cat&quot; : { &quot;name&quot; : &quot;Mario&quot;, &quot;age&quot; : 11 }, &quot;dog&quot; : { &quot;name&quot; : &quot;Teresa&quot;, &quot;age&quot; : 11 } }
{ &quot;cat&quot; : { &quot;name&quot; : &quot;Dario&quot;, &quot;age&quot; : 9 } }
{ &quot;cat&quot; : { &quot;name&quot; : &quot;Giorgia&quot;, &quot;age&quot; : 16 } }
{ &quot;cat&quot; : { &quot;name&quot; : &quot;Cristian&quot;, &quot;age&quot; : 2 } }
{ &quot;cat&quot; : { &quot;name&quot; : &quot;Stefano&quot;, &quot;age&quot; : 3 } }
{ &quot;cat&quot; : { &quot;name&quot; : &quot;Silvia&quot;, &quot;age&quot; : 17 }, &quot;dog&quot; : { &quot;name&quot; : &quot;Emanuele&quot;, &quot;age&quot; : 0 } }
{ &quot;cat&quot; : { &quot;name&quot; : &quot;Elisa&quot;, &quot;age&quot; : 4 }, &quot;dog&quot; : { &quot;name&quot; : &quot;Giulia&quot;, &quot;age&quot; : 5 } }
{ &quot;cat&quot; : { &quot;name&quot; : &quot;Rita&quot;, &quot;age&quot; : 14 } }
{ &quot;cat&quot; : { &quot;name&quot; : &quot;Veronica&quot;, &quot;age&quot; : 10 }, &quot;dog&quot; : { &quot;name&quot; : &quot;Mauro&quot;, &quot;age&quot; : 5 } }
{ &quot;dog&quot; : { &quot;name&quot; : &quot;Silvia&quot;, &quot;age&quot; : 12 } }
{ &quot;dog&quot; : { &quot;name&quot; : &quot;Patrizia&quot;, &quot;age&quot; : 2 } }
{ &quot;cat&quot; : { &quot;name&quot; : &quot;Eleonora&quot;, &quot;age&quot; : 17 }, &quot;dog&quot; : { &quot;name&quot; : &quot;Angela&quot;, &quot;age&quot; : 6 } }
{ &quot;cat&quot; : { &quot;name&quot; : &quot;Angela&quot;, &quot;age&quot; : 3 } }
{ &quot;cat&quot; : { &quot;name&quot; : &quot;Gianluca&quot;, &quot;age&quot; : 12 }, &quot;dog&quot; : { &quot;name&quot; : &quot;Giovanna&quot;, &quot;age&quot; : 12 } }
{ &quot;dog&quot; : { &quot;name&quot; : &quot;Mario&quot;, &quot;age&quot; : 17 } }
{ &quot;dog&quot; : { &quot;name&quot; : &quot;Elena&quot;, &quot;age&quot; : 10 } }
{ &quot;cat&quot; : { &quot;name&quot; : &quot;Giovanna&quot;, &quot;age&quot; : 16 }, &quot;dog&quot; : { &quot;name&quot; : &quot;Roberto&quot;, &quot;age&quot; : 14 } }
{ &quot;cat&quot; : { &quot;name&quot; : &quot;Giuseppe&quot;, &quot;age&quot; : 13 } }
Type &quot;it&quot; for more</pre>
12. Quais são as 5 pessoas mais velhas com sobrenome Rossi?
<pre>&gt; db.italians.find({&quot;surname&quot;: &quot;Rossi&quot;},{&quot;name&quot;:1,&quot;surname&quot;:1,&quot;age&quot;:1}).pretty().limit(5).sort({age : -1})
{
	&quot;_id&quot; : ObjectId(&quot;5e95ffb2ea9cd5fe9f79437a&quot;),
	&quot;surname&quot; : &quot;Rossi&quot;,
	&quot;age&quot; : 79
}
{
	&quot;_id&quot; : ObjectId(&quot;5e95ffadea9cd5fe9f7922ee&quot;),
	&quot;surname&quot; : &quot;Rossi&quot;,
	&quot;age&quot; : 77
}
{
	&quot;_id&quot; : ObjectId(&quot;5e95ffb0ea9cd5fe9f79357f&quot;),
	&quot;surname&quot; : &quot;Rossi&quot;,
	&quot;age&quot; : 77
}
{
	&quot;_id&quot; : ObjectId(&quot;5e95ffb0ea9cd5fe9f7937f1&quot;),
	&quot;surname&quot; : &quot;Rossi&quot;,
	&quot;age&quot; : 77
}
{
	&quot;_id&quot; : ObjectId(&quot;5e95ffb2ea9cd5fe9f794122&quot;),
	&quot;surname&quot; : &quot;Rossi&quot;,
	&quot;age&quot; : 77
}
</pre>
13. Crie um italiano que tenha um leão como animal de estimação. Associe um nome e idade ao bichano
<pre>&gt; db.italians.insert({
... &quot;firstname&quot; : &quot;Marco&quot;,
... &quot;surname&quot; : &quot;Batista&quot;,
... &quot;username&quot; : &quot;marcoab&quot;,
... &quot;age&quot; : 35,
... &quot;email&quot; : &quot;marcoabatista@gmail.com&quot;,
... &quot;bloodType&quot; : &quot;O-&quot;,
... &quot;id_num&quot; : &quot;123456789001&quot;,
... &quot;registerDate&quot; : new Date(&quot;2020/04/14&quot;),
... &quot;ticketNumber&quot; : 13456789,
... &quot;jobs&quot; : [ &quot;Consultor&quot; ],
... &quot;favFruits&quot; : [ &quot;Laranja&quot; ],
... &quot;movies&quot; : [ { &quot;title&quot; : &quot;Seven: Os Sete Crimes Capitais (1995)&quot;, &quot;rating&quot; : 3.86 } ],
... &quot;lion&quot; : { &quot;name&quot; : &quot;Pacato&quot;, &quot;age&quot; : 5 } })
WriteResult({ &quot;nInserted&quot; : 1 })
</pre>
14. Infelizmente o Leão comeu o italiano. Remova essa pessoa usando o Id.
<pre>&gt; db.italians.find({&quot;firstname&quot;:&quot;Marco&quot;, &quot;surname&quot;:&quot;Batista&quot;},{&quot;_id&quot;:1})
{ &quot;_id&quot; : ObjectId(&quot;5e961847f3c1c2f4ea96ebb1&quot;) }
&gt; db.italians.remove({&quot;_id&quot; : ObjectId(&quot;5e961847f3c1c2f4ea96ebb1&quot;)})
WriteResult({ &quot;nRemoved&quot; : 1 })
</pre>
15. Passou um ano. Atualize a idade de todos os italianos e dos bichanos em 1.
<pre>&gt; db.italians.update({}, {&quot;$inc&quot;: {&quot;age&quot;: 1}}, {multi: true});
WriteResult({ &quot;nMatched&quot; : 10000, &quot;nUpserted&quot; : 0, &quot;nModified&quot; : 10000 })
&gt; db.italians.update({dog: { $exists: true}}, {$inc :{&quot;dog.age&quot; : 1}}, {multi: true})
WriteResult({ &quot;nMatched&quot; : 3993, &quot;nUpserted&quot; : 0, &quot;nModified&quot; : 3993 })
&gt; db.italians.update({cat: { $exists: true}}, {$inc :{&quot;dog.age&quot; : 1}}, {multi: true})
WriteResult({ &quot;nMatched&quot; : 6003, &quot;nUpserted&quot; : 0, &quot;nModified&quot; : 6003 })
</pre>
16. O Corona Vírus chegou na Itália e misteriosamente atingiu pessoas somente com gatos e de 66 anos. Remova esses italianos.
<pre>&gt; db.italians.remove({cat: { $exists: true}, &quot;age&quot; : 66});
WriteResult({ &quot;nRemoved&quot; : 70 })
</pre>
17. Utilizando o framework agregate, liste apenas as pessoas com nomes iguais a sua respectiva mãe e que tenha gato ou cachorro.
18. Utilizando aggregate framework, faça uma lista de nomes única de nomes. Faça isso usando apenas o primeiro nome
19. Agora faça a mesma lista do item acima, considerando nome completo.
20. Procure pessoas que gosta de Banana ou Maçã, tenham cachorro ou gato, mais de 20 e menos de 60 anos.
<pre>db.italians.find({$and: [{$or: [{&quot;favFruits&quot; : &quot;Banana&quot;}, {&quot;favFruits&quot; : &quot;Maçã&quot;}]}, {$or: [{dog: {$exists: true}}, {cat:{ $exists: true}} ]}, {&quot;age&quot; : {&quot;$gt&quot; : 20, &quot;$lt&quot; : 60}}]}).count()
1298
</pre>


