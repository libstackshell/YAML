# ScriptStack.YAML

Einfaches Plugin zum Arbeiten mit YAML Daten

```YAML
---
# A sample yaml file
company: spacelift
domain:
 - devops
 - devsecops
tutorial:
  - yaml:
      name: "YAML Ain't Markup Language"
      type: awesome
      born: 2001
  - json:
      name: JavaScript Object Notation
      type: great
      born: 2001
  - xml:
      name: Extensible Markup Language
      type: good
      born: 1996
author: omkarbirade
published: true
```

```Javascript
function main() {

    // -------- Datei lesen --------
    var f = std.fopen("test.yaml", "r");
    var data = std.fread(f);
    std.fclose(f);

    println("== RAW YAML ==\n" + data + "\n");

    // -------- parse --------
    var doc = yaml.parse(data);
	

    // -------- get / has --------
    println("company exists? " + yaml.has(doc, "company"));
    println("company = " + yaml.get(doc, "company"));

    println("domain[0] exists? " + yaml.has(doc, "domain[0]"));
    println("domain[0] = " + yaml.get(doc, "domain[0]"));

    // nested example 
    println("tutorial[0].yaml.name = " + yaml.get(doc, "tutorial[0].yaml.name"));

    // -------- count --------
    println("root count (mapping keys) = " + yaml.count(doc));
    println("domain count (sequence) = " + yaml.count(yaml.get(doc, "domain")));

    // -------- keys --------
    var k = yaml.keys(doc);
    println(k); 

    // -------- type checks --------
    println("doc is map? " + yaml.is_map(doc));
    println("doc is seq? " + yaml.is_seq(doc));
    println("domain is seq? " + yaml.is_seq(yaml.get(doc, "domain")));

    // -------- iter root mapping --------
    var it = yaml.iter(doc);
    while (yaml.next(it)) {
        // mapping: index = -1, key != null
        println("key=" + yaml.key(it) + " idx=" + yaml.index(it) + " val=" + yaml.value(it));
    }

    // -------- iter sequence (domain) --------
    it = yaml.iter(yaml.get(doc, "domain"));
    while (yaml.next(it)) {
        // sequence: key = null, index >= 0
        println("idx=" + yaml.index(it) + " val=" + yaml.value(it));
    }

    // -------- set: einfache Werte --------
    yaml.set(doc, "company", "NEWCO");
    yaml.set(doc, "published", false);
    yaml.set(doc, "domain[1]", "security");
    println("company now = " + yaml.get(doc, "company"));
    println("published now = " + yaml.get(doc, "published"));
    println("domain[1] now = " + yaml.get(doc, "domain[1]"));

    // -------- set: ArrayList -> YAML Sequence --------
    // [1,2,3] als ScriptStack ArrayList 
    yaml.set(doc, "numbers", [1, 2, 3]);
    println("numbers count = " + yaml.count(yaml.get(doc, "numbers")));
    it = yaml.iter(yaml.get(doc, "numbers"));
    while (yaml.next(it)) {
        println("numbers[" + yaml.index(it) + "]=" + yaml.value(it));
    }

    // -------- yaml_node / yaml_string --------
    var node = yaml.node([ "a", "b", "c" ]);     // macht Sequence
    println(yaml.string(node));

    // -------- final dump --------
    println("== FINAL YAML ==\n" + yaml.string(doc));

    it = yaml.iter(doc);
	  while (yaml.next(it)) {
	      var idx = yaml.index(it);
	      if (idx >= 0)
		        println("[" + idx + "] => " + yaml.value(it));
	      else
		        println(yaml.key(it) + " => " + yaml.value(it));
    }

}
```
