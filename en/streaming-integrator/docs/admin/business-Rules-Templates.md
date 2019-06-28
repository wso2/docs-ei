# Business Rules Templates

Rule Templates are used as specifications to gain inputs from users
through dynamically generated fields, for creating business rules. A
template group is a business domain level grouping. The definition of a
template looks as follows.

``` js
    {
      "templateGroup" : {
        "name" : "<Name of the template group>",
        "uuid":"<UUID for the template group>",
        "description" : "<(Optional) description for the template group>",
        "ruleTemplates" : [
          {
            "name" : "<Name of the rule template>" ,
            "uuid" : "<UUID for the rule template>",
            "type" : "template",
            "instanceCount" : "one <or> many",
            "description" : "<(Optional) description for the rule template>",
            "script" : "<(Optional) Javascript with reference to the properties>",
            "templates" : [
              { "type" : "siddhiApp",
                "content" : "<SiddhiApp_1 with ${templatedProperty_x}>"
              },
              { "type" : "siddhiApp",
                "content" : "<SiddhiApp_n with ${templatedProperty_y}>"
              }
            ],
            "properties" : {
                "templatedProperty_x" : {"fieldName" : "<Field name for the property>", "description" : "<Description for the property>", "defaultValue" : "<Default value for the property>"},
                "templatedProperty_y" : {"fieldName" : "<Field name for the property>", "description" : "<Description for the property>", "defaultValue" : "<Default value for the property>", "options" : ["<option_1>", "<option_n>"]}
            }
          },
          {
            "name" : "<Name of the rule template>",
            "uuid" : "<UUID for the rule template>",
            "type" : "input",
            "instanceCount" : "one <or> many",
            "description" : "<(Optional) description for the rule template>",
            "script" : "<(Optional) Javascript with reference to the properties>",
            "templates" : [
              { "type" : "siddhiApp",
                "content" : "<SiddhiApp with ${templatedProperty_x}>",
                "exposedStreamDefinition" :"<Exposed stream definition>"
              }
            ],
            "properties" : {
              "templatedProperty_x" : {"fieldName" : "<Field name for the property>", "description" : "<Description for the property>", "defaultValue" : "<Default value for the property>", "options" : ["<option_1>", "<option_n>"]}
            }
          },
          {
            "name" : "<Name of the rule template>",
            "uuid" : "<UUID for the rule template>",
            "type" : "output",
            "instanceCount" : "one <or> many",
            "description" : "<(Optional) description for the rule template>",
            "script" : "<(Optional) Javascript with reference to the properties>",
            "templates" : [
              { "type" : "siddhiApp",
                "content" : "<SiddhiApp with ${templatedProperty_x}>",
                "exposedStreamDefinition" :"<Exposed stream definition>"
              }
            ],
            "properties" : {
              "templatedProperty_x" : {"fieldName" : "<Field name for the property>", "description" : "<Description for the property>", "defaultValue" : "<Default value for the property>", "options" : ["<option_1>", "<option_n>"]}
            }
          }
        ]
      }
    }
```

The following parameters are configured:

### **Template Group basic data**

The following parameters are configured under
`         templateGroup        ` .

<table>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Description</th>
<th>Required/Optional</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><code>             name            </code></td>
<td>A name for the template group<br />
</td>
<td>Required</td>
</tr>
<tr class="even">
<td><code>             uuid            </code></td>
<td>A uniquely identifiable id for the template group</td>
<td>Required</td>
</tr>
<tr class="odd">
<td><code>             description            </code></td>
<td>A description for the template.</td>
<td>Optional</td>
</tr>
</tbody>
</table>

### **Rule Template details**

Multiple rule templates can be defined under a
`         templateGroup        ` . For each
`         ruleTemplate        ` , the following set of parameters need
to be configured:

<table>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Description</th>
<th>Required/Optional</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><code>             name            </code></td>
<td>A name for the rule template</td>
<td>Required</td>
</tr>
<tr class="even">
<td><code>             uuid            </code></td>
<td>A uniquely identifiable id for the rule template</td>
<td>Required</td>
</tr>
<tr class="odd">
<td><code>             type            </code></td>
<td>The type of the rule template. Possible values are as follows:
<ul>
<li><strong><code>                template:               </code></strong> Used only to create an entire business rule from template</li>
<li><strong><code>                input :               </code></strong> Used only in creating a business rule from scratch</li>
<li><strong><code>                output :               </code></strong> Used only in creating a business rule from scratch <code>                             </code></li>
</ul></td>
<td>Required</td>
</tr>
<tr class="even">
<td><code>             instanceCount            </code></td>
<td><p>This specifies whether the business rules derived from the template can be deployed only on one node, or whether they can be deployed on many nodes.</p>
<p>Possible values are as follows:</p>
<ul>
<li><strong>one</strong></li>
<li><strong>many</strong></li>
</ul></td>
<td>Required</td>
</tr>
<tr class="odd">
<td><code>             script            </code></td>
<td><div class="content-wrapper">
The Java script to be executed on the templated fields.<br />
Developers can use this script for:
<ul>
<li>validating purposes.</li>
<li>deriving values for a templated parameter by combining some other entered parameters</li>
</ul>
<p>Each templated element that is going to be derived from entered parameters, has to be mentioned as a variable in the global scope of the javascript.</p>
<p>The entered parameters should be templated in the script itself, and will be later replaced with their respective entered values.</p>
<p>Consider the following script</p>
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: js; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: js; gutter: false; theme: Confluence"><pre class="sourceCode js"><code class="sourceCode javascript"><span id="cb1-1"><a href="#cb1-1"></a><span class="co">/* </span></span>
<span id="cb1-2"><a href="#cb1-2"></a><span class="co">* Validates a number and returns after adding 10 to it</span></span>
<span id="cb1-3"><a href="#cb1-3"></a><span class="co">* @throws Error when a non number is entered</span></span>
<span id="cb1-4"><a href="#cb1-4"></a><span class="co">*/</span></span>
<span id="cb1-5"><a href="#cb1-5"></a><span class="kw">function</span> <span class="at">deriveValue</span>(value)<span class="op">{</span></span>
<span id="cb1-6"><a href="#cb1-6"></a>    <span class="cf">if</span>( <span class="op">!</span><span class="at">isNan</span>(value) ) <span class="op">{</span></span>
<span id="cb1-7"><a href="#cb1-7"></a>      <span class="cf">return</span> value <span class="op">+</span> <span class="dv">10</span><span class="op">;</span></span>
<span id="cb1-8"><a href="#cb1-8"></a>    <span class="op">}</span></span>
<span id="cb1-9"><a href="#cb1-9"></a>    <span class="cf">throw</span> <span class="st">&quot;A number is required&quot;</span><span class="op">;</span></span>
<span id="cb1-10"><a href="#cb1-10"></a><span class="op">}</span></span>
<span id="cb1-11"><a href="#cb1-11"></a></span>
<span id="cb1-12"><a href="#cb1-12"></a><span class="kw">var</span> derivedValue <span class="op">=</span> <span class="at">deriveValue</span>($<span class="op">{</span>enteredValue<span class="op">}</span>)<span class="op">;</span></span></code></pre></div>
</div>
</div>
<p><code>                             </code></p>
<p><code>               enteredValue              </code> should be defined as a property under <code>               properties              </code> in order to be filled by the user and replaced later.</p>
<p>The derived value stored in <code>               derivedValue              </code> will be then used to replace <code>               ${derivedValue              </code> } in the SiddhiApp template.<br />
<br />
</p>
</div></td>
<td>Optional</td>
</tr>
<tr class="even">
<td><code>             description            </code></td>
<td>A brief description of the rule template.</td>
<td>Optional</td>
</tr>
<tr class="odd">
<td><code>             templates            </code></td>
<td><p>These are the artifacts (i.e SiddhiApps) with templated parameters, that will be instantiated with replaced values when a business rule is created.</p></td>
<td>Required</td>
</tr>
<tr class="even">
<td><code>             properties            </code></td>
<td><p>You can add a field name, description, default value and possible values (optional)<br />
for the templated parameters.</p></td>
<td>Required</td>
</tr>
</tbody>
</table>

  

1.  Save the template group you created as a
    `           .json          ` file in the
    `           <SP_HOME>/wso2/dashboard/resources/businessRules/templates          `
    directory.

2.  In the `           BusinessRules          ` section of the
    `           <SP_HOME>/conf/dashboard/deployment.yaml          `
    file, add a configuration for the template you created as shown
    below.

    ``` java
            wso2.business.rules.manager:
        
              datasource: <datasourceName>
                - nodeURL1:
                   - ruleTemplateUUID1
                   - ruleTemplateUUID2
                  nodeURL2:
                   - ruleTemplateUUID1
                   - ruleTemplateUUID2
    ```

        !!! info
    
        If you add this configuration, the business rules template is
        deployed only in the specified nodes when you run the worker and
        dashboard servers of your SP setup. If you do not add this
        configuration, the template is deployed in all the worker nodes of
        your SP set up.
    
