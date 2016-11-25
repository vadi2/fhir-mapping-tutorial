# FHIR tutorial mapping example
FHIR has a mapping language to convert between logical models an a nice [tutorial](http://build.fhir.org/mapping-tutorial.html) how use it, this projects tries to build the structure definitions and mappings for it.

ATTENTION: work in progress, to be run the fhir source from gforge has to checked out and run. there need to be  patches to be applied to the reference implementation, will submitted them after the tutorial works on my side


## setup

eclipse based configuration

```
import projects fhir/build/implementations/java/org.hl7.fhir.dstu3
import projects fhir/build/implementations/java/org.hl7.fhir.utilities
```

check out this project into your eclipse workspace and adjust the paths in
src/ch/ahdis/fhir/dstu3/test/TutorialMapTests.java


## run the tutorial
for each step there is a directory below the maptutorial directory

logical: structurdefinitions for tleft, tright
map: maps
source: files used to transform
target: result of map transform (will be generated by the unittests)

run the unit test ...

## work in progress - questions

### newbie question

should in step#1 of the mapping tutorial the element a be created with the rule
"rule_a" : for source.a as a make target.a = a if target.a is not defined?

## org.hl7.fhir.dstu3.elementmodel.Element.setProperty - not done yet. 
a rough first implementation is with the patch in the directory

## issues step1 - if value is an xml element not an attribute

Open issue step1: how to specify that in the source is 

```xml
<a>test</a> 
```

and not 

```xml
<a value="test" />
```

when used the StructureDefinition below the map rule is working but with 

```xml
<TLeft>test</Tleft> 
```
and not 

```xml
<TLeft><a>test</a></TLeft>
```

```xml
<StructureDefinition xmlns="http://hl7.org/fhir">
	<id value="TLeft" />
	<extension
		url="http://hl7.org/fhir/StructureDefinition/elementdefinition-namespace">
		<valueUri value="http://hl7.org/fhir/tutorial" />
	</extension>
	<meta>
		<lastUpdated value="2016-11-21T16:11:04.708+01:00" />
	</meta>
	<url value="http://hl7.org/fhir/StructureDefinition/tutorial-left" />
	<name value="TLeft" />
	<status value="draft" />
	<kind value="logical" />
	<abstract value="false" />
	<snapshot>
		<element>
			<path value="TLeft" />
			<min value="1" />
			<max value="1" />
		</element>
		<element>
			<path value="TLeft.a" />
			<representation value="xmlText"/>
			<min value="1" />
			<max value="1" />
			<type>
				<code value="string" />
			</type>
		</element>
	</snapshot>
</StructureDefinition>
```

## issues step3

```
"rule_a20b" : for source.a2 as a where a1.length <= 20 make target.a2 = a // ignore it
"rule_a20c" : for source.a2 as a check a2.length <= 20 make target.a2 = a // error if it's longer than 
```

change it to:
```

rule_a20a: for source.a2 as a make target.a2 = truncate(a, 20)
rule_a20b: for source.a2 as a where a2.length() <= 20 make target.a2 = a
rule_a20c: for source.a2 as a check a2.length() <= 20 make target.a2 = a
```
afther the where, check clause the name of the element has to be used (a2), after the make the variable 


## issues step4

Transfrom not supported yet.
