# lxml

1. Parsing an XML/HTML document: `tree = etree.parse('filename.xml')`​​
2. Finding elements by tag name: `tree.xpath('//tagname')`​​
3. Finding elements by attribute value: `tree.xpath('//tagname[@attribute="value"]')`​​
4. Finding elements by class name: `tree.xpath('//tagname[@class="classname"]')`​​
5. Finding the text content of an element: `element.text`​​
6. Finding the value of an attribute: `element.get('attribute')`​​
7. Finding the parent element: `element.getparent()`​​
8. Finding the children elements: `element.getchildren()`​​
9. Modifying the text content of an element: `element.text = 'new text'`​​
10. Modifying the value of an attribute: `element.set('attribute', 'new value')`​​
11. Adding a new element: `new_element = etree.SubElement(parent_element, 'tagname')`​​
12. Adding a new attribute to an element: `element.set('new_attribute', 'value')`​​
13. Removing an element: `parent_element.remove(element)`​​
14. Removing an attribute from an element: `element.attrib.pop('attribute')`​​
15. Checking if an element has children: `len(element.getchildren()) > 0`​​
16. Checking if an element has a specific attribute: `'attribute' in element.attrib`​​
17. Checking if an element has a specific tag name: `element.tag == 'tagname'`​​
18. Checking if an element has a specific text content: `element.text == 'text'`​​
19. Checking if an element has a specific attribute value: `element.get('attribute') == 'value'`​​
20. Checking if an element has a specific class name: `'classname' in element.get('class').split()`​​
21. Iterating over elements with a specific tag name: `for element in tree.iter('tagname'):`​​
22. Iterating over elements with a specific attribute value: `for element in tree.xpath('//tagname[@attribute="value"]'):`​​
23. Iterating over elements with a specific class name: `for element in tree.xpath('//tagname[contains(@class, "classname")]'):`​​
24. Getting the root element of a document: `tree.getroot()`​​
25. Getting the tag name of an element: `element.tag`​​
26. Getting the attributes of an element: `element.attrib`​​
27. Getting the text content of all child elements: `element.xpath('string()')`​​
28. Getting the text content of all descendant elements: `element.xpath('.//text()')`​​
29. Getting the inner HTML of an element: `etree.tostring(element, method='html', with_tail=False).decode()`​​
30. Getting the outer HTML of an element: `etree.tostring(element, method='html', with_tail=True).decode()`​​
