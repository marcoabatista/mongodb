# mongodb

Atividade Prática – MongoDB

Exercício 1- Aquecendo com os pets

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
