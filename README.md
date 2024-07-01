# Lecture 5. Database design. Normalization and Normal Forms

<h3>Abstract</h3>

<p>In lecture 5 we consider the quality of the database schema.  This quality is
defined by the theory of normal forms.  The notion of normal forms is based
on the mathematical model of relations and functional dependencies. To understand this
part you have to know basic notions of discrete mathematics like set, map and relation.</p>

<hr><h3><a name="Pozadane">Required properties of the data model</a></h3>

<p>The following properties of the data model must be established
at the very beginning
of the design process.  It can hardly be achieved without cooperation with the users:</p>

<dl>
<dt><a name="correctness"><i>correctness</i></a>
<dd>The model must conform to reality.
<dt><a name="relevance"><i>relevance</i></a>
<dd>Every item in the model must be relevant to the operation of the organization.
<dt><a name="completness"><i>completeness</i></a>
<dd>No entity relevant to the operation of the organization is missing.
</dl>

<p>After the data model had been developed,</p>

<ul>
<li>users should understand it and after the detailed analysis they should formally
        approve it with full responsibility;
<li>the designer of the database should believe that the approved data model faithfully
        reflects the reality and he/she should build the database and the database application
        which conform to that model.
</ul>

<hr><h3><a name="Normalizacja">Normalization</a></h3>

<p>The normalization requirement can be expressed as follows:</p>

<table><tr><td class="important">
Every fact stored in a database should be expressible in only one way.
</tr></table>

<p>Here are some examples:</p>

<ul>
<li>The information on a lecture should be stored only once and not with all
        the students who attend this lecture.
<li>If we store the information on parents of each person, we need not store
        the information on grandparents, because you can deduce it from the data
        on parents.
<li>If the model contains separate entities <i>Passenger</i> and <i>Employee</i>,
        the data on one person may be stored twice in the database: once as a passenger
        and once as an employee of the airline.
</ul>

<p>A presentation of the normalization process is based on the formal mathematical
model of relations.  We will introduce it in the second part of the lecture.
However we start from simple intuitive examples which illustrate the paradigms of
normalization.</p>

<hr><h3><a name="Dlaczego">Why are some schemata ill-formed?</a></h3>

<p>The disadvantages of ill-formed schemata will be presented by
two examples.  The primary key is highlighted with boldface.</p>

<h4>Example 1</h4>

<p>Suppliers = { <b>Supplier_name</b>, Address, <b>Product_name</b>, Price }</p>


<table border="1" align="center">
<tr><th>Supplier_name<th>Address<th>Product_name<th>Price

<tr>    <td><font color="green">Smith</font>
        <td><font color="green">7, Violin str.</font>
        <td>TV set
        <td>1500
<tr>    <td><font color="green">Smith</font>
        <td><font color="green">7, Violin str.</font>
        <td>Radio receiver
        <td>500
<tr>    <td><font color="brown">Blake</font>
        <td><font color="brown">5, Mozart str.</font>
        <td>TV set
        <td>1800
<tr>    <td><font color="brown">Blake</font>
        <td><font color="brown">5, Mozart str.</font>
        <td>Computer
        <td>5000
<tr>    <td><font color="green">Smith</font>
        <td><font color="green">7, Violin str.</font>
        <td bgcolor="#CCFFCC">Battery
        <td>5
<tr>    <td>Conway
        <td>140, Warsaw str.
        <td>VCR
        <td>1000
</table>

<p>Here are the disadvantages of this schema which can be easily noticed:</p>

<dl>
<dt><a name="anomaly"><i>redundancy</i></a>
<dd>The address of a supplier is repeated with every product delivered by
        this supplier.
<dt><i>update anomaly</i>
<dd>If we update an address in one place, it retains the old value
       in other rows.
<dt><i>insert anomaly</i>
<dd>You can hardly insert a supplier that does not supply anything yet.
        The name of the product belongs to primary key, thus it cannot contain NULL.
<dt><i>delete anomaly</i>
<dd>If you delete the information on all products supplied by a supplier,
       at the same time you will remove all the information on the supplier.
</dl>

<p>These drawbacks occur because in this schema two separate objects
were combined into one entity.  It would be better to create two tables:</p>

<ul>
<li>Suppliers = { <b>Supplier_name</b>, Address }
<li>Products = { <b>Supplier_name</b>, <b>Product_name</b>, Price }
</ul>

<p>In order to correct the schema, simply break the initial table into
two tables which represent products and suppliers respectively.</p>

<h4>Example 2</h4>

<p>Employees = { <b>Empno</b>, Name, School_name, Address }</p>

<table border="1" align="center">
<tr><th>Empno<th>Name<th>School_name<th>Address
<tr>    <td>101
        <td>Smith
        <td><font color="green">PJWSTK</font>
        <td><font color="green">86, Koszykowa str.</font>
<tr>    <td>123
        <td>Jones
        <td><font color="brown">MIMUW</font>
        <td><font color="brown">2, Banach str.</font>
<tr>    <td>109
        <td>Blake
        <td><font color="brown">MIMUW</font>
        <td><font color="brown">2, Banach str.</font>
<tr>    <td>102
        <td>Miller
        <td><font color="green">PJWSTK</font>
        <td><font color="green">86, Koszykowa str.</font>
<tr>    <td>105
        <td>Clark
        <td><font color="brown">MIMUW</font>
        <td><font color="brown">2, Banach str.</font>
</table>

<p>Again we can observe similar disadvantages in spite of the fact
that the primary key contains only one column.</p>

<dl>
<dt><i>redundancy</i>
<dd>The address of a school is repeated with each of its employees.
<dt><i>update anomaly</i>
<dd>If we update an address in one place, it retains the old value
       in other rows.
<dt><i>insert anomaly</i>
<dd>You can hardly insert a school which has no employees.
        Column <i>Empno</i> is the primary key, thus it cannot be NULL.
<dt><i>delete anomaly</i>
<dd>If you delete the information on all employees of a school,
       you will remove all the information on the school at the same time.
</dl>

<p>Again these drawbacks occur because in this schema two separate objects
were combined into one entity.  It would be better to create two tables:</p>

<ul>
<li>Employees = { <b>Empno</b>, Name, School_name }
<li>Schools = { <b>School_name</b>, Address }
</ul>

<p>In order to correct the schema, simply break the initial table into
two tables which represent respectively employees and schools.</p>

<table><tr><td  class="notec">
<p>Consider schema <i>Hospital</i> =
{ Patient, Sickness, Doctor, Register, Record, Address } </p>

<p>The following rules hold:</p>

<ol>
<li>Every patient has a register.
<li>Every register contains an address.
<li>A register contains records.
<li>A record in a register concerns a sickness.
<li>A record in a register is created by a doctor.
</ol>

<p>Describe the redundancies and anomalies in this schema.</p>
</tr></table>

<h4><a name="Wyjasnienie">Partial and transitive dependencies</a></h4>

<p><a name="Dla">In</a> the schema:</p>

<p>Suppliers = { <b>Supplier_name</b>, Address, <b>Product_name</b>, Price }</p>

<ul>
<li>the key is the pair of attributes <b>Supplier_name</b> and <b>Product_name</b>,
<li><i>Address</i> depends on a part of the key, namely on <b>Supplier_name</b>.
</ul>

<p>We say that the value of the attribute <i>Address</i> <i>partially</i> depends on the key:</p>

<pre>Supplier_name &rarr; Address</pre>

<p>Such a dependency is called <i>partial</i>.  After we move attributes
<b>Supplier_name</b> and <i>Address</i> to a separate entity, attribute
<b>Supplier_name</b> becomes the key of the new entity and partial dependency
<code>Supplier_name &rarr; Address</code> is a dependency on the whole key.</p>

<p align="center"><img border="0" src="https://gakko.pjwstk.edu.pl/materialy/2398/lec/wyklad05/images/5_1.png"></p>

<p><a name="Dla">In</a> the schema:</p>

<p>Employees = { <b>Empno</b>, Name, School_name, Address }</p>

<ul>
<li>the key is the attribute <b>Empno</b>,
<li><i>Address</i> depends on the attribute <i>School_name</i>, which does not belong to the key.
</ul>

<p>We say that the value of the attribute <i>Address</i> <i>transitively</i>
depends on the key:</p>

<pre>School_name &rarr; Address</pre>

<p>Such a dependency is called <i>transitive</i>.  After we move the attributes
<b>School_name</b> and <i>Address</i> to a separate entity, attribute
<b>School_name</b> becomes the key of the new entity and the partial dependency
<code>School_name &rarr; Address</code> is the dependency on the whole key.</p>

<p align="center"><img border="0" src="https://gakko.pjwstk.edu.pl/materialy/2398/lec/wyklad05/images/5_2.png"></p>

<p>Therefore we can say that the existence of partial or transitive
dependencies indicates that the schema is ill-formed.  The only acceptable dependencies
are dependencies on the whole key.</p>

<hr><h3><a name="Formalny">Formal model of relational databases</a></h3>

<ul>
<li><i>A relation</i> is an abstract mathematical artifact which is the
	basis for
        the relational model. <i>A relational database</i> is a set of relations.
<li><i>A table</i> is an actual appearance of a relation.  A relation has many
        different appearances in the forms of tables.
<li>In a relation the order of columns and rows does not matter.
<li>Two rows of a table which contain the same values are treated as identical, i.e.
        as the same element of the relation.
</ul>

<h4>Example relation - Flights</h4>

<table border="1" align="center">
<tr><th>Flight_no<th>From<th>To<th>Departure<th>Arrival
<tr><td> 83<td>Warsaw   <td>Moscow      <td>11:30<td>13:43
<tr><td> 84<td>Moscow   <td>Warsaw      <td>15:00<td>17:55
<tr><td>109<td>Warsaw   <td>New York    <td>09:50<td>16:52
<tr><td>213<td>Warsaw   <td>Frankfurt   <td>11:43<td>12:45
<tr><td>214<td>Frankfurt<td>Warsaw      <td>14:20<td>15:29
<tr><td>115<td>New York <td>Warsaw      <td>18:12<td>07:10
<tr><td>515<td>New York <td>Frankfurt   <td>22:00<td>09:15
<tr><td>516<td>Frankfurt<td>New York    <td>13:20<td>19:15
<tr><td>711<td>Warsaw   <td>Tokyo       <td>18:00<td>09:10
</table>

<p>If we change the order of rows and columns in this table, we will still have the same
relation.</p>

<h4><a name="Schematx">Schema</a></h4>

<p><i>The schema of a relation</i> is a set:

<blockquote><i>R</i> = { <i>A</i><sub>1</sub>, <i>A</i><sub>2</sub>, ...., <i>A</i><sub><i>n</i></sub> }</blockquote>

where <i>A</i><sub>1</sub>, <i>A</i><sub>2</sub>, ...., <i>A</i><sub><i>n</i></sub>
are attributes (names of columns). For instance,

<blockquote> <i>Flights</i> = { Flight_no, From, To, Departure, Arrival }<br>
<i>Employee</i> = { Empno, Name, Deptno, Sal, Job } <br>
<i>Department </i> = { Deptno, Name, Location }
</blockquote>

</p>

<h4>Domain of an attribute</h4>

<p><i>The domain of an attribute</i> is the set of allowed values of this attribute.
We denote it be <i>Dom</i>.  For instance:

<blockquote>
<i>Dom</i>(Flight_no) = <code>NUMBER(3)</code><br>
<i>Dom</i>(From) = <code>VARCHAR(15)</code><br>
<i>Dom</i>(To) = <code>VARCHAR(15)</code><br>
<i>Dom</i>(Departure) = <code>CHAR(5)</code><br>
<i>Dom</i>(Arrival) = <code>CHAR(5)</code>
</blockquote>

</p>

<h4>Domain of a relation</h4>

<p><i>The domain of a relation</i> of schema
<i>R</i> = { <i>A</i><sub>1</sub>, <i>A</i><sub>2</sub>, ...., <i>A</i><sub><i>n</i></sub> }
is the union of the domains of its attributes:

<blockquote><i>Dom</i>(<i>R</i>) =
<i>Dom</i>(<i>A</i><sub>1</sub>) &cup;
<i>Dom</i>(<i>A</i><sub>2</sub>) &cup; .. &cup;
<i>Dom</i>(<i>A</i><sub><i>n</i></sub>)</blockquote>

where &cup; denotes the union of sets.</p>

<h4><a name="Relacja">Relation</a></h4>

<p><i>A relation</i> of schema
<i>R</i> = { <i>A</i><sub>1</sub>, <i>A</i><sub>2</sub>, ...., <i>A</i><sub><i>n</i></sub> }
is a finite set of mappings:

<blockquote><i>r</i> = {<i>t</i><sub>1</sub>, <i>t</i><sub>2</sub>, ..., <i>t</i><sub><i>k</i></sub> }</blockquote>

where each mapping

<blockquote><i>t</i><sub><i>i</i></sub> : <i>R</i> &rarr; <i>Dom</i>(<i>R</i>)</blockquote>

satisfies the condition that

<blockquote><i>t</i><sub><i>i</i></sub>(<i>A</i><sub><i>j</i></sub>) &isin;
        <i>Dom</i>(<i>A</i><sub><i>j</i></sub>)</blockquote>
</p>

<p>Each such mapping is called <i>a tuple</i>(or <i>a row</i>).</p>

<h4>Example of a tuple</h5>

<p>A tuple corresponds to a row (a record) of a table. We describe it formally, when we
indicate values for its attributes, e.g.</p>

<blockquote>
<i>t</i>(Flight_no) = 83<br>
<i>t</i>(From) = "Warsaw"<br>
<i>t</i>(To) = "Moscow"<br>
<i>t</i>(Departure) = "11:30"<br>
<i>t</i>(Arrival) = "13:43"<br>
</blockquote>

<p>we can also do it graphically:</p>

<table border="1" align="center">
<tr><th>Flight_no<th>From<th>To<th>Departure<th>Arrival
<tr><td> 83<td>Warsaw   <td>Moscow      <td>11:30<td>13:43
</table>

<h4>Restriction of a tuple</h4>

<p><i>The restriction of tuple</i> <i>t</i> of relation of schema
<i>R</i> = { <i>A</i><sub>1</sub>, <i>A</i><sub>2</sub>, ...., <i>A</i><sub><i>n</i></sub> }
to set <i>X</i> &sub; <i>R</i> is denoted by:

<blockquote><i>t</i>|<sub><i>X</i></sub> : <i>X</i> &rarr; <i>Dom</i>(<i>R</i>)</blockquote>

and satisfies the condition that:

<blockquote><i>t</i>|<sub><i>X</i></sub>(<i>A</i><sub><i>i</i></sub>) =
<i>t</i>(<i>A</i><sub><i>i</i></sub>)</blockquote>

for all  <i>A</i><sub><i>i</i></sub> &isin; <i>X</i>.</p>

<p>For <i>A</i><sub><i>j</i></sub> that do not belong to <i>X</i> the value of
<i>t</i>|<sub><i></i></sub>(<i>A</i><sub><i>j</i></sub>) is undefined.</p>

<p>For instance, if

<blockquote><i>X</i> = { From, To }</blockquote>

for tuple <i>t</i> from the previous example, it holds that:</p>

<blockquote>
<i>t</i>|<sub><i>X</i></sub>(From)="Warsaw" <br>
<i>t</i>|<sub><i>X</i></sub>(To) = "Moscow"
</blockquote>

<p>Graphically:</p>

<table border="1" align="center">
<tr><th>From<th>To
<tr><td>Warsaw   <td>Moscow
</table>

<h4><a name="Zalez">Functional dependency</a></h4>

<p>A relation of schema
<i>R</i> = { <i>A</i><sub>1</sub>, <i>A</i><sub>2</sub>, ...., <i>A</i><sub><i>n</i></sub> }
satisfies <i>functional dependency</i>

<blockquote><i>X</i> &rarr; <i>Y</i></blockquote>

if for each pair of tuples <i>t</i> and <i>u</i> the following condition is true:

<blockquote>
if <i>t</i>|<sub><i>X</i></sub>  = <i>u</i>|<sub><i>X</i></sub>   then
<i>t</i>|<sub><i>Y</i></sub>  = <i>u</i>|<sub><i>Y</i></sub>
</blockquote>

That means that in this relation the values of the attributes which belong to set <i>X</i>
uniquely determine the values of the attributes which belong to set <i>Y</i>.</p>

<p>In the relation <i>Flights</i> the value of the attribute <i>Flight_no</i> uniquely identifies
the whole flight, thus it uniquely determines the values of all attributes of the relation.
Therefore, the following functional dependency holds:</p>

<blockquote>Flight_no &rarr; From, To, Departure, Arrival</blockquote>

<p>Let us analyze one more example with functional 
dependencies among attributes. This is
the relation <i>ZodiacSigns</i> with attributes
<i>Id</i>, <i>FirstName</i>, <i>SecondName</i>, <i>BirthDate</i> and <i>ZodiacSign</i>.
</p>

<table border="1" align="center">
<tr>    <th>Id  <th>FirstName   <th>SecondName  <th>BithDate    <th>ZodiacSign
<tr>    <td>1   <td>Agnes       <td>Jones       <td>23.01.1990  <td>Aquarius
<tr>    <td>2   <td>Mary        <td>Blake       <td>01.04.2000  <td>Aries
<tr>    <td>3   <td>Chris       <td>Anderson    <td>23.04.1956  <td>Taurus
<tr>    <td>4   <td>Cameron     <td>Smith       <td>13.04.2000  <td>Aries
<tr>    <td>5   <td>Pamela      <td>Miller      <td>31.07.2000  <td>Leo
<tr>    <td>6   <td>George      <td>Powell      <td>05.09.2000  <td>Virgo
<tr>    <td>7   <td>Gregory     <td>Rice        <td>12.07.1971  <td>Cancer
</table>

<p>This relation satisfies the following functional dependency:

<blockquote>BirthDate &rarr; ZodiacSign</blockquote>

(the same birth dates correspond to the same zodiac signs).</p>

<p>In fact it is not only a functional dependency but also a known function
which maps the day of birth to the zodiac sign.</p>

<h4>Identifying functional dependencies</h4>

<p>In the design process of a database we have to identify functional dependencies
for every schema.  They can be different for different applications.</p>

<p>For instance in relation <i>Flights</i> we can identify the following set of
functional dependencies:

<blockquote>{ Flight_no } &rarr; { From, To, Departure, Arrival }<br>
{ From, To, Departure } &rarr; { Flight_no, Arrival }<br>
{ From, To, Arrival } &rarr; { Flight_no, Departure }
</blockquote>

Usually we use a shorter notation which consists in omitting commas and curly
brackets:

<blockquote>Flight_no &rarr; From To Departure Arrival <br>
From To Departure &rarr; Flight_no Arrival <br>
From To Arrival &rarr; Flight_no Departure
</blockquote>
</p>

<h4><a name="Nadkl">Superkey</a></h4>

<p><i>A superkey</i> of a relation of schema
<i>R</i> = { <i>A</i><sub>1</sub>, <i>A</i><sub>2</sub>, ...., <i>A</i><sub><i>n</i></sub> }
is a set attributes <i>S</i> &sub; <i>R</i> such that the following dependency
is satisfied:

<blockquote><i>S</i> &rarr; <i>R</i></blockquote>

The value of any attribute must be uniquely determined by the values of the attributes
of set <i>S</i>.  At least one superkey always exists.  It is the set of all attributes
(<i>R</i>).</p>

<h4><a name="klucz">Key</a></h4>

<p><i>A key</i> of a relation of schema
<i>R</i> = { <i>A</i><sub>1</sub>, <i>A</i><sub>2</sub>, ...., <i>A</i><sub><i>n</i></sub> }
is a set attributes <i>K</i> which is a minimal superkey (i.e. it does not include
any other superkey).</p>

<p>That means that the values of all attributes in <i>R</i> are uniquely determined
by the values of the attributes of set <i>K</i> and no proper subset of <i>K</i> has
this property.</p>

<p>There always exists at least one key, because there is always at least one superkey.
The number of keys may be greater than one. For example in the relation <i>Flights</i>
we can deduce three keys from the set of functional dependencies:

<blockquote>{ Flight_no }<br>
{ From, To, Departure }<br>
{ From, To, Arrival }
</blockquote>


<h4>Keys and the primary key</h4>

<p>Among the keys of a relation we distinguish one <i>primary key</i>.
Its attributes are highlighted with boldface or underlined.</p>

<p>In the example relation <i>Flights</i> we choose <b>Flight_no</b> as the primary key:</p>

<blockquote> <i>Flights</i> = { <b>Flight_no</b>, From, To, Departure, Arrival }</blockquote>

<p>We encourage the reader to work on the following exercise:</p>

<table><tr><td  class="notec">
<p>Consider the schema <i>Hospital</i> =
{ Patient, Sickness, Doctor, Register, Record, Address } </p>

<p>The following rules hold:</p>

<ol>
<li>Every patient has a register.
<li>Every register contains an address.
<li>A register contains records.
<li>A record in a register concerns a sickness.
<li>A record in a register is created by a doctor.
</ol>

<p>
<a href="javascript:popUp('ok1.html',400,310)">What</a>
functional dependencies does this schema satisfy?
What is the key? Is the number of keys greater than one?
</p>

</tr></table>

<h4>Semantics of functional dependencies</h4>

<p>A dependency on something else than a key introduces an internal
relation among the attributes of a table. It causes that values of some attributes
can be determined by the values of other attributes.  This is the redundancy.
In the following table dependency <i>X</i> &rarr; <i>Y</i> is satisfied:
</p>

<table border="1" align="center">
<tr><th><i>X</i><th>...<th><i>Y</i>     <th>...
<tr><td>...     <td>...<td>...          <td>...
<tr>    <td><font color="green">xxx</font>
        <td>...
        <td><font color="brown">yyy</font>
        <td>...
<tr><td>...     <td>...<td>...          <td>...
<tr>    <td><font color="green">xxx</font>
        <td>...
        <td><font color="brown">???</font>
        <td>...
<tr><td>...     <td>...<td>...          <td>...
</table>


<p>If <i>X</i> is not a superkey, this table contains redundancy.
The value in the cell marked with <font color="brown">???</font> is already
uniquely determined. It must be <font color="brown">yyy</font>.
If <i>X</i> were a superkey, that table could not look that way. Two rows with the same
value of all columns of the superkey could not exist.</p>

<h4>Non-key dependencies</h4>

<p>Functional dependency <i>X</i> &rarr; <i>Y</i> is <i>a key dependency</i> if
<i>X</i> is a superkey.</p>

<p>Functional dependency <i>X</i> &rarr; <i>Y</i> is <i>a non-key dependency</i> if:</p>

<ol>
<li>it is not trivial, i.e. <i>Y</i> is not a subset of <i>X</i>.
<li><i>X</i> is not a superkey.
</ol>

<p><a name="Zalezki">There are two types of non-key dependencies:</a>

<ol>
<li><i>partial dependencies</i> = non-key dependencies on a part of a key,
<li><i>transitive dependencies</i> = non-key dependencies on something else than a part of a key.
</ol>

<p>In the schemata <i>Suppliers</i> and <i>Employees</i> there are two non-key dependencies:</p>

<blockquote>(partial dependency) Supplier_name &rarr; Address</blockquote>

<blockquote>(transitive dependency) School_name &rarr; Address</blockquote>

<table><tr><td  class="notec">
<p>Consider the schema <i>Hospital</i> =
{ Patient, Sickness, Doctor, Register, Record, Address }. In this schema we have identified
the following functional dependencies:</p>

<ol>
<li>Patient &rarr; Register
<li>Register &rarr; Address
<li>Record &rarr; Register
<li>Record &rarr; Sickness
<li>Record &rarr; Doctor
</ol>

<p>The only key is the pair of attributes <b>Patient</b> and <b>Record</b>.</p>

<p><a href="javascript:popUp('ok2.html',420,320)">Please classify</a>
these functional dependencies.</p>
</td></tr></table>

<hr><h3><a name="Metoda eliminowania">Eliminating non-key dependencies</a></h3>

<p>This method consists in the transfer of the non-key dependency (either partial or
transitive) to a separate table and the removal of right-hand side attributes of the dependency
from the original table.</p>

<p>In the case of the schema <i>Suppliers</i> we add a new table
{ <b>Supplier_name</b>, <i>Address</i> } and we remove <i>Address</i> from the
original table, which now takes the form of { <b>Supplier_name</b>, <b>Product_name</b>,
<i>Price</i> }:

<p align="center"><img border="0" src="https://gakko.pjwstk.edu.pl/materialy/2398/lec/wyklad05/images/5_1.png"></p>

<p>In the case of the schema <i>Employees</i> we add a new table
{ <b>School_name</b>, <i>Address</i> } and we remove <i>Address</i> from the
original table, which now takes the form of 
{ <b>Empno</b>, <b>Name</b>, <i>School_name</i> }:

<p align="center"><img border="0" src="https://gakko.pjwstk.edu.pl/materialy/2398/lec/wyklad05/images/5_2.png"></p>

<p>This way the functional dependencies determine the list of tables that
should occur in the proper schema. Each non-trivial dependency should be stored
in a separate table.</p>

<table><tr><td  class="notec">
<p><a href="javascript:popUp('ok3.html',450,130)">Use this elimination method</a>
and transform the schema <i>Hospital</i> into correct relational schemata.
</td></tr></table>

<hr><h3><a name="Posta">Boyce-Codd normal form</a></h3>

<p>A relation with schema <i>R</i> is BCNF (Boyce-Codd normal form) if it has no non-key
dependencies, i.e. for each dependency <i>X</i> &rarr; <i>A</i> in schema  <i>R</i>
(<i>X</i> &sub; <i>R</i>, <i>A</i> &isin; <i>R</i>) it holds that:

<ol>
<li><i>A</i> &isin; <i>X</i> (a trivial dependency) or
<li><i>X</i> is a superkey.
</ol>

<p>If a schema is BCNF, it is impossible to deduce any values from other values.
However, we still have no guarantee that there are no redundancies. We will show
it in the point devoted to <a href="#Czwarta">4NF</a> and <a href="#Piata">5NF</a>.</p>

<h4>Examples of BCNF schemata</h4>

<ol>
<li><i>R</i> = { Empno, Name, Job, Sal }

<blockquote>Empno &rarr; Name Job Sal</blockquote>

<li><i>R</i> = { Flight_no, From, To, Departure, Arrival }

<blockquote>Flight_no &rarr; From To Departure Arrival<br>
From To Departure &rarr; Flight_no Arrival<br>
From To Arrival &rarr; Flight_no Departure
</blockquote>

</ol>

<h4><a name="Schemat">A schema which cannot be made BCNF</a></h4>

<p>Some schemata cannot be converted to a set of BCNF tables without the loss
of information and functional dependencies. An example of such a schema is
<i>CSZ</i> = { <i>City</i>, <i>Street</i>, <i>Zip</i> } with dependencies:

<blockquote>
Street City &rarr; Zip<br/>
Zip &rarr; City
</blockquote>

This schema contains two keys:

<blockquote>
{ Street, City }<br/>
{ Zip, City }
</blockquote>

Because of dependency <i>Zip</i> &rarr; <i>City</i> this schema is not BCNF. However,
it cannot be broken without the loss of functional dependencies because one of the
dependencies contains all attributes.</p>

<hr><h3><a name="Trzecia">Third normal form</a></h3>

<p><i>A key attribute</i> is any attribute which belongs to at least one key.</p>


<p>A relation with the schema <i>R</i> is 3NF (third normal form) if
all non-key dependencies have key attributes on the right-hand side, i.e.
for each dependency <i>X</i> &rarr; <i>A</i> in schema  <i>R</i>
(<i>X</i> &sub; <i>R</i>, <i>A</i> &isin; <i>R</i>) it holds that:

<ol>
<li><i>A</i> &isin; <i>X</i> (a trivial dependency) or
<li><i>X</i> is a superkey.
<li><i>A</i> is a key attribute.
</ol>

Thus 3NF excludes functional dependencies:

<blockquote><i>X</i> &rarr; <i>A</i></blockquote>

such that <i>A</i> is not a key attribute, <i>X</i> is not a superkey
and <i>A</i> is not a member of <i>X</i>.  When you want to achieve 3NF and
you are about to break a schema into
two tables, you have to look for such dependencies. </p>


<table><tr><td class="przyk">
<p>Functional dependency <i>Zip</i> &rarr; <i>City</i> in schema
<a href="#Schemat">CSZ</a> indicates that this schema is not BCNF. It is however 3NF
because <i>City</i> is a key attribute. It belongs to one of the keys, namely
{ <i>City</i>, <i>Street</i> }.</p>

<p>The following schema is not 3NF:

<blockquote>
<i>R</i> = { <i>A</i>, <i>B</i>, <i>C</i>, <i>D</i> }
<i>AB</i> &rarr; <i>C</i>;  <i>B</i> &rarr;<i> D</i>; <i>BC</i> &rarr; <i>A</i>
</blockquote>
The keys are { <i>A</i>, <i>B</i> } and { <i>B</i>, <i>C</i> }, thus
the dependency <i>B</i> &rarr; <i>D</i> make it non-3NF (<i>D</i> is not a key attribute and
<i>B</i> is not a superkey).</p>
</table>

<p>The absence of "ill" functional dependencies does not guarantee that there are no
redundancies and anomalies. We will present two more kinds of dependencies:
<i>multivalued dependencies</i> and <i>join dependencies</i> which cause
redundancies and anomalies. We will show some examples but we will skip the
formal definitions.</p>

<hr><h3><a name="Czwarta">Fourth normal form</a></h3>

<p>Take a look at the following table:</p>

<table border="1" align="center">
<tr><th>Stud_id <th>Lecture     <th>Sport
<tr><td>100     <td>Databases   <td>Tennis
<tr><td>100     <td>Databases   <td>High jump
<tr><td>100     <td>Algebra     <td>Tennis
<tr><td>100     <td>Algebra     <td>High jump
<tr><td>200     <td>Databases   <td>Long jump
</table>

<p>This relation is BCNF because the only key contains all three attributes. However
the table contains redundancies and allows anomalies.</p>

<h4>Multivalued dependency</h4>

<p>Schema <i>R</i> = { <i>Stud_id</i>, <i>Lecture</i>, <i>Sport</i> } there are so called
<i>multivalued dependencies</i>:

<blockquote>Stud_id -&gt;&gt; Lecture<br>
Stud_id -&gt;&gt; Sport
</blockquote>

<h4>4NF</h4>

<p>A schema is <i>4NF</i> (fourth normal form) if all multivalued dependencies are on
superkeys.</p>

<p>The above schema is not 4NF then.</p>

<p>To eliminate these multivalued dependencies and convert the schema to 4NF, you have
to break it into two relations: { <i>Stud_id</i>, <i>Lecture</i> }
and { <i>Stud_id</i>, <i>Sport</i> }.</p>

<hr><h3><a name="Piata">Fifth normal form</a></h3>

<h4>Join dependencies</h4>

<p>A join dependency is a generalization of the multivalued dependency in the
sense that the join dependency shows the way to break the schema into more
pieces than two.</p>

<p>Consider the following relation among <i>Suppliers</i>,
<i>Products</i> and <i>Enterprises</i>:</p>

<table border="1" align="center">
<tr>    <th>Supplier    <th>Product     <th>Enterprise
<tr>    <td><font color="green">Paris</font>
        <td><font color="green">Apples</font>
        <td>Zeus
<tr>    <td>Menelaus
        <td><font color="green">Apples</font>
        <td><font color="green">Aphrodite</font>
<tr>    <td><font color="green">Paris</font>
        <td>Nuts
        <td><font color="green">Aphrodite</font>
<tr>    <td><font color="red">Paris</font>
        <td><font color="red">Apples</font>
        <td><font color="red">Aphrodite</font>
<tr>    <td>Priam       <td>Apples      <td>Zeus
<tr>    <td>Priam       <td>Peaches     <td>Mars
<tr>    <td>Agamemnon<td>Peaches        <td>Apollo
</table>

<p>The following business rule holds:

<dl>
<dt><font color="green">if</font>
<dd>    <ol>
        <li>supplier <font color="green"><i>S</i></font> supplies product <font color="green"><i>P</i></font> for some enterprise, and
        <li>supplier <font color="green"><i>S</i></font> supplies some product for enterprise <font color="green"><i>E</i></font> <br>
                (i.e. supplier <font color="green"><i>S</i></font> works for enterprise <font color="green"><i>E</i></font>), and
        <li>some supplier supplies product <font color="green"><i>P</i></font> for enterprise <font color="green"><i>E</i></font> <br>
                (i.e. enterprise <font color="green"><i>E</i></font> needs product <font color="green"><i>P</i></font>),
        </ol>
<dt><font color="red">then</font>
<dd>    <ol start="4">
        <li>supplier <font color="red"><i>S</i></font> supplies product <font color="red"><i>P</i></font> for enterprise <font color="red"><i>E</i></font>.
</dl>

Therefore there is a redundancy, because

<dl>
<dt><font color="green">since</font>
<dd>    <ol>
        <li><font color="green"><i>Paris</i></font> supplies <font color="green"><i>apples</i></font> for some enterprise, and
        <li><font color="green"><i>Paris</i></font> supplies some product for <font color="green"><i>Aphrodite</i></font><br>
                (i.e. <font color="green"><i>Paris</i></font> works for <font color="green"><i>Aphrodite</i></font>), and
        <li>some supplier supplies <font color="green"><i>apples</i></font> for <font color="green"><i>Aphrodite</i></font><br>
                (i.e. <font color="green"><i>Aphrodite</i></font> needs <font color="green"><i>apples</i></font>),
        </ol>
<dt><font color="red">then</font>
<dd>    <ol start="4">
        <li>supplier <font color="red"><i>Paris</i></font> supplies product <font color="red"><i>Apples</i></font> for enterprise <font color="red"><i>Aphrodite</i></font>.
</dl>

<h4>Elimination of this redundancy</h4>

<p>The redundancy (i.e. the join dependency) can be eliminated by
the partition of the table into three parts:

<blockquote>
<i>Needed</i> = { Product, Enterprise }<br>
<i>Works_for</i> = { Supplier, Enterprise }<br>
<i>Can_supply</i> = { Supplier, Product }
</blockquote>

If we create only two relations, we will lose information.  If we have three, we can join
them to get the original relation.</p>

<p>However, if the original table stored also the quantity of each product supplied by
a supplier for an enterprise, such partition could not be made, because it would mean
the loss of information. </p>

<hr><h3><a name="Granice">Limits to normalization</a></h3>

<p>Sometimes the normalization is not performed completely.  For example
some functions of the database application may prefer unnormalized schemata. In particular
it is true for data warehouses.</p>

<p>If we do not complete the normalization, we have to be aware of all the consequences
of this decision.  In such cases all non-key dependences must be checked
at each update.  Furthermore all remaining anomalies must be mitigated, e.g. by means
of additional tables which handle insertions and updates.
</p>

<hr><h3><a name="Podsumowanie">Summary</a></h3>

<p>During the work on the data model and the database schema the designer
must take into account many issues like the correctness, the relevance
and the completeness of the data model. The designed schema must not imply
redundancies and anomalies during insertions, updates and deletions. In rare cases we
accept some redundancies, but it must be the result of a deliberate action and
must be accompanied by appropriate protection against update anomalies.</p>

<p>We have briefly presented the theoretical foundations of normalization.
They are based on functional dependencies among attributes of relations.
We have defined Boyce-Codd normal form and the third normal form.  We have also
mentioned fourth and fifth normal forms which guarantee that the schema of the
database has no redundancies and implies no anomalies caused by "ill" dependencies
of disparate kinds (functional, multivalued and join).
</p>

<hr><h3><a name="Slownik">Dictionary</a></h3>

<dl>

<dt><a href="#anomaly">anomaly</a>
<dd>Loss of information, disintegration of data or impossibility 
	to perform an operation caused by the ill-formed schema of a database.
<dt><a href="#Posta">Boyce-Codd normal formal</a> (BCNF)
<dd>The condition on a schema
	that there are no non-key dependencies.
<dt><a href="#completness">completeness</a> of a data model
<dd>The property of the data model which contains all entities relevant 
	to the operation of the organization.
<dt><a href="#correctness">correctness</a> of a data model
<dd>The property of the data model which 
	conforms to the reality.
<dt><a href="#Zalez">functional dependency</a>
<dd>A dependency of values of some attributes on the values of other attributes.
	It is denoted by <i>X</i> &rarr; <i>Y</i> which reads "<i>X</i> determines <i>Y</i>".
<dt><a href="#klucz">key</a>
<dd>A minimal set of attributes of a relation whose values uniquely determine the values of
	all other attributes of the relation.
<dt><a href="#Normalizacja">normalization</a>
<dd>The process which leads to the database schema 
	that is in appropriate normal form.
<dt><a href="#Zalezki">partial dependency</a>
<dd>A functional dependency on 
	a part of a key.
<dt><a href="#Relacja">relation</a>
<dd>A mathematical object (a set of tuples)
	that is the abstraction of a database table.
<dt><a href="#relevance">relevance</a> of a data model
<dd>The property of the data model 
	which conforms to reality.
<dt><a href="#Schematx">schema</a> of a relation
<dd>The list of attributes
	of this relation.
<dt><a href="#Nadkl">superkey</a>
<dd>A set of attributes of a relation whose values uniquely determine the values of
	all other attributes of the relation.
<dt><a href="#Trzecia">third normal form</a> (3NF)
<dd>The condition on a schema
	that there are no non-key dependencies which determine non-key attributes.
<dt><a href="#Zalezki">transitive dependency</a>
<dd>A functional dependency on a set of attributes that is incomparable (in the sense 
	of set inclusion) with any key.
<dt><a href="#Relacja">tuple</a>
<dd>A mathematical object (a mapping) which is the abstraction 
	of a database row.
</dl>

<hr><h3><a name="Zadania">Exercises</a></h3>

<ol>
<li>You are given the following schema:
	
	<blockquote><i>R</i> = { Stockbroker, BrokerageHouse, Investor, 
		Asset, Stock, Dividend }
</blockquote>

and the following functional dependencies:

<blockquote>
Asset &rarr; Dividend<br>
Investor &rarr; Stockbroker<br>
Investor Asset &rarr; Stock<br>
Stockbroker  &rarr; BrokerageHouse
</blockquote>

	<ol type="a">
	<li>Describe redundancies and anomalies which are present in this schema.
	<li>Does the following dependency hold?
		<blockquote>
			Investor Dividend &rarr; BrokerageHouse
		</blockquote>
	<li>Find all keys.
	<li>Convert this schema to 3NF without any loss of information and functional dependencies.
	<li>Are all obtained schemata BCNF? Did any redundancies and anomalies remain?
	</ol>
<li>In a textbook for farmers who keep ostriches a database schema is proposed.
This schema contains only one table:

<blockquote><i>R</i> = { OstrichPenId, OstrichCount, OstrichName, OstrichSex, OstrichAge, KeeperId, KeeperName }
</blockquote>

The following functional dependencies hold:

<blockquote>
	OstrichPenId &rarr; OstrichCount		<br>
	OstrichPenId &rarr; KeeperId			<br>
	OstrichName OstrichPenId &rarr; OstrichSex	<br>
	OstrichName OstrichPenId &rarr; OstrichAge	<br>
	KeeperId	&rarr;	KeeperName
</blockquote>

<p>Explain the drawbacks of this schema to you friend who plans to breed
ostriches.
Show him/her a better schema which can be used to store the same information.
</p>
</ol>
