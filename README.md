LIN (Lazy Information Notation)
===

LIN is a family of open data notation specifications (with some reference implementations) designed to make it easy for both humans and automated systems to create, read and transfer.

It is created primarily to replace and standardize text-based configuration files across computer systems for Lightwisters' internal systems, in the future, the specification may also include Java Object Serialisation.

Table of Content
---
1. [Current Status](#status) 
2. [Motivation & Inspirations](#motivation) 
3. [Features](#features) 
4. [Variants](#variants) 
5. [Future Work](#future) 
6. [License](#license) 

<a name="status"></a>Current Status
---
The project is currently an early stage experimental stage and is considered not safe to use in production.

<a name="motivation"></a>Motivation & Inspirations
---
We started LIN because we're unhappy with the way most configuration files are written. 

There are two emerging trends that we are trying to deal with.
1. As systems become more complex, the use of text-based configuration files becomes larger.
2. Complex systems are more and more dependent on a diverse range of sub-systems, each written by different individuals with preferences to language and configuration parsing.

One can conclude there is a growing demand to lessen the burden of creating, editing and maintaining text-based configurations for such systems.

A typical configuration file in PHP will often look something like this...

```PHP
$config['default']['language']='en';
$config['default']['timezone']='Asia/Hong_Kong';
$config['default']['database']['core'][0]['host']='localhost';
$config['default']['database']['core'][0]['type']='mysql';
$config['default']['database']['core'][0]['db']='mydb';
$config['default']['database']['core'][0]['port']='3306';
$config['default']['database']['core'][0]['username']='myusername';
$config['default']['database']['core'][0]['password']='password';
...
$config['default']['database']['core'][1]['host']='server2.domain.net';
...
$config['default']['database']['backup'][0]['host']='backup.domain.net';
...
```

In XML, which many Java applications still rely on today it looks something like this...

```XML
<managed-bean>
	<managed-bean-name>contactController</managed-bean-name>
	<managed-bean-class>
              com.arcmind.contact.controller.ContactController
         </managed-bean-class>
	<managed-bean-scope>session</managed-bean-scope>
	<managed-property>
		<property-name>contactRepository</property-name>
		<property-class>
                   com.arcmind.contact.model.ContactRepository
                </property-class>
		<value>#{contactRepository}</value>
	</managed-property>
...
```
In JSON, which is seeing increasing adoption of choice in certain areas, it looks a little something like this...

```JSON
{
    "default":{
		"language":"en",
		"timezone":"Asia/Hong_Kong",
		"database":{
			"core":[
				{
					"host":"localhost",
                    "type":"mysql",
					"db":"mydb",
					"port":"3306",
					"username":"root",
					"password":"password"
				},
                {
                    "host":"server2.domain.net",
                    ...
                }
			],
            "backup":[
                {
                    "host":"backup.domain.net",
                    ...
                }
            ]
		},
````

And in Linux/Unix services often have their own text based parsers to parse configuration syntax, making configuration files application specific. 

Each of the cases above involves bad practises that is listed in the programmer's guide book. **Don't Repeat Yourselves (DRY)** principle is well-known and established, as is **Keep It Simple, Stupid (KISS)**.

Our objectives with LIN is simple.
- Language and application independence.
- Allow anyone to create configuration files without repeating useless syntax.
- Keep syntax to a minimum to maximise readability.
- Allow configurations to be streamed, compressed and encrypted for machine-to-machine communications.

In short, we want human programmers to work less to achieve the same result, and machines to use less resources when synchronising configurations.

We want to be lazy. This is what you can expect to write using LIN

```lin
config > default > language = en
>> timezone = Asia/Hong_Kong
```
This is the simpliest example where 3 fields are created on line 1, each assigned under the previous declared field and the last field is assigned a value of "en".

On the 2nd line, a new field is created that is assigned to the pervious "2nd level" field, which in this case is "default" and is then assigned the value "Asia/Hong_Kong"

Essentially LIN is a "smart" top-down notation language where fields are spit by levels, previous field names are remembered and default values can be assigned to arrays of similar configurations.

A database array configuration under LIN can look something like this...

```lin
database[]
> host = [localhost] [server2.domain.net]
> type = mysql
> db = mydb
> port 3306
> username = [root] [server2user]
> password = [password] [moreSecurePassword]
```

What happened in the above declaration is that we defined a field named "database" on line 1 that is expected to be an array.

In subsequent lines we define 2nd level fields that belongs to "database" and specified the values.

We used 2 different type of notations on the subsequent lines to specify values belonging to the database array. On the 2nd line, the `database[i].host` field value is specified by the use of [] brackets. On the 3rd, we specify a _default_ value for all array "objects" we've declared within scope.

That is it for a short sneak peak. We will be writing a more solid specification in time and we will create examples to illustrate a lot more advanced and complex use cases that allows users to remain _DRY_ while ensuring readbility is not compromised.

<a name="features"></a>Features
---
- UTF-8 by default for multi-lingual support
- Stream compatible, and compressible and encryptable
- DRY (Don't Repeat Yourself) notations
- Interchangable(Interoperable) variants
- Easily readable text variant syntax
- Efficient binary variant for machines

<a name="variants"></a>Variants
---
Due to the need to optimize for 2 different audiences and the ability to be interoperable between machines and human readers, there are currently 2 confirmed variants.

**LINT** (Lazy Information Notation Text) is a pure text based notation specification designed for maximum productivity and readability for human users.

**LINB** (Lazy Information Notation Binary) is a pure binary based specification for machines.

<a name="future"></a>Future Work
---
We are currently pondering the idea to extend LIN to language dependent object serialisation, we will be developing LIN 1.0 with this in mind, but under no circumstances will our primary objectives be compromised.

If we deem that perhaps a different variant of LIN is required to extend it for object serialisation then we may do so.

<a name="involved"></a>Get Involved
---
We opensourced our specification and reference implementation with the hope that the wider world can help us achieve what we believe is a common objective, as such please feel free to critique or chip-in to the development of this project to make it better.

<a name="license"></a>License
---
The project is released under the MIT license, please look at the License file in the repository.
