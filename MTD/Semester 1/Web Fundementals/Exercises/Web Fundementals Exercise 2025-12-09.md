#web_fundementals 

**[Github Repo](https://github.com/Web-Fundamentals-WS-2025-26/ue07-ZoeMcFife)**

i just realised i misspelled fundamentals …lol 


Verbose as FUCK

```xsd
<?xml version="1.0" encoding="UTF-8"?>  
<xs:schema elementFormDefault="qualified"  
           xmlns:xs="http://www.w3.org/2001/XMLSchema">  
    <xs:element name="university">  
        <xs:complexType>  
            <xs:sequence>  
                <xs:element name="lecturers">  
                    <xs:complexType>  
                        <xs:sequence>  
                            <xs:element name="lecturer" maxOccurs="unbounded" minOccurs="0">  
                                <xs:complexType>  
                                    <xs:sequence>  
                                        <xs:element type="xs:string" name="name"/>  
                                        <xs:element type="xs:string" name="email"/>  
                                    </xs:sequence>  
                                    <xs:attribute type="xs:ID" name="id" use="required"/>  
                                </xs:complexType>  
                            </xs:element>  
                        </xs:sequence>  
                    </xs:complexType>  
                </xs:element>  
                <xs:element name="courses">  
                    <xs:complexType>  
                        <xs:sequence>  
                            <xs:element name="course" maxOccurs="unbounded" minOccurs="0">  
                                <xs:complexType>  
                                    <xs:sequence>  
                                        <xs:element type="xs:string" name="subject"/>  
                                        <xs:element type="xs:decimal" name="ects"/>  
                                        <xs:element type="xs:string" name="contents"/>  
                                        <xs:element name="instructors">  
                                            <xs:complexType>  
                                                <xs:sequence>  
                                                    <xs:element name="instructor" maxOccurs="unbounded">  
                                                        <xs:complexType>  
                                                            <xs:attribute type="xs:IDREF" name="ref" use="required"/>  
                                                        </xs:complexType>  
                                                    </xs:element>  
                                                </xs:sequence>  
                                            </xs:complexType>  
                                        </xs:element>  
                                    </xs:sequence>  
                                    <xs:attribute type="xs:string" name="code" use="required"/>  
                                </xs:complexType>  
                            </xs:element>  
                        </xs:sequence>  
                    </xs:complexType>  
                </xs:element>  
            </xs:sequence>  
        </xs:complexType>  
    </xs:element>  
</xs:schema>
```
why odes the formatting break what

ok idc stay ugly xml stay ugly
```XML
<?xml version="1.0" encoding="utf-8" ?>  
  
<university xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="university.xsd">  
    <lecturers>        <lecturer id="s">  
            <name>Bingus Dingus</name>  
            <email>bingus.dingus@fungus.com</email>  
        </lecturer>        <lecturer id="d">  
            <name>Oingo Boingo</name>  
            <email>oingo.boingo@fungus.com</email>  
        </lecturer>    </lecturers>    <courses>        <course code="HD">  
            <subject>House Dorito</subject>  
            <ects>125</ects>  
            <contents>404</contents>  
            <instructors>                <instructor ref="s"/>  
            </instructors>        </course>        <course code="SW">  
            <subject>Cat goes meow</subject>  
            <ects>1</ects>  
            <contents>404</contents>  
            <instructors>                <instructor ref="d"/>  
                <instructor ref="s"/>  
            </instructors>        </course>    </courses></university>
```

```JSON
{  
  "lecturers":  
  [  
    {  
      "id": "1",  
      "name": "Baaaaaaaaaaaaaaaaaaaaaaaaaaaaaa",  
      "email": "booooooooooooooooooooooooooooooo"  
    },  
    {  
      "id": "2",  
      "name": "2",  
      "email": "43"  
    }  
  ],  
  "courses":  
  [  
    {  
      "code": "dgdgdgdgdg",  
      "subject": "dgdfgf",  
      "ects": 243425,  
      "contents": "dggdgdgdgdgdgdgdgdgdgdgdgdgd",  
      "instructor_ids":  
      [  
        "1",  
        "2"  
      ]  
    }  
  ]  
}
```

```YAML
lecturers:  
  - email: test  
    name: tes  
    id: idw  
  - email: safd  
    name: dgd  
    id: gfdf  
courses:  
  - subject: dgdfhf  
    code: dgd  
    ects: 34  
    contents: ddhfdhdfhd  
    instructor_ids:  
      - idw  
      - gfdf
```

Yaml is so cute! i should use it more often 


like json is sooooooooooooooooo ugly



![[Pasted image 20251209141030.png]]

also YAML stands for:  *YAML Ain’t Markup Language*?

i thought it was *Yet Another Markup Language* but i think that was just a programmer joke that went over my head lol

tbh it is kinda funny 

