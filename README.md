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
    var f = fopen("test.yaml", "r");
    var data = fread(f);
    fclose(f);

    print("== RAW YAML ==\n" + data + "\n");

    // -------- parse --------
    var doc = yaml.parse(data);

    // -------- get / has --------
    print("company exists? " + yaml.has(doc, "company"));
    print("company = " + yaml.get(doc, "company"));

    print("domain[0] exists? " + yaml.has(doc, "domain[0]"));
    print("domain[0] = " + yaml.get(doc, "domain[0]"));

    // nested example 
    print("tutorial[0].yaml.name = " + yaml.get(doc, "tutorial[0].yaml.name"));

    // -------- count --------
    print("root count (mapping keys) = " + yaml.count(doc));
    print("domain count (sequence) = " + yaml.count(yaml.get(doc, "domain")));

    // -------- keys --------
    var k = yaml.keys(doc);
    print(k); 

    // -------- type checks --------
    print("doc is map? " + yaml.is_map(doc));
    print("doc is seq? " + yaml.is_seq(doc));
    print("domain is seq? " + yaml.is_seq(yaml.get(doc, "domain")));

    // -------- iter root mapping --------
    var it = yaml.iter(doc);
    while (yaml.next(it)) {
        // mapping: index = -1, key != null
        print("key=" + yaml.key(it) + " idx=" + yaml.index(it) + " val=" + yaml.value(it));
    }

    // -------- iter sequence (domain) --------
    it = yaml.iter(yaml.get(doc, "domain"));
    while (yaml.next(it)) {
        // sequence: key = null, index >= 0
        print("idx=" + yaml.index(it) + " val=" + yaml.value(it));
    }

    // -------- set: einfache Werte --------
    yaml.set(doc, "company", "NEWCO");
    yaml.set(doc, "published", false);
    yaml.set(doc, "domain[1]", "security");
    print("company now = " + yaml.get(doc, "company"));
    print("published now = " + yaml.get(doc, "published"));
    print("domain[1] now = " + yaml.get(doc, "domain[1]"));

    // -------- set: ArrayList -> YAML Sequence --------
    // [1,2,3] als ScriptStack ArrayList 
    yaml.set(doc, "numbers", [1, 2, 3]);
    print("numbers count = " + yaml.count(yaml.get(doc, "numbers")));
    it = yaml.iter(yaml.get(doc, "numbers"));
    while (yaml.next(it)) {
        print("numbers[" + yaml.index(it) + "]=" + yaml.value(it));
    }

    // -------- yaml_node / yaml_string --------
    var node = yaml.node([ "a", "b", "c" ]);     // macht Sequence
    print(yaml.string(node));

    // -------- final dump --------
    print("== FINAL YAML ==\n" + yaml.string(doc));

    it = yaml.iter(doc);
	  while (yaml.next(it)) {
	      var idx = yaml.index(it);
	      if (idx >= 0)
		        print("[" + idx + "] => " + yaml.value(it));
	      else
		        print(yaml_key(it) + " => " + yaml.value(it));
    }

}
```
