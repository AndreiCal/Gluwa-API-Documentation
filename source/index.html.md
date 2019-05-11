### Bank

#### Description
Bank API provides the list of financial institutions that Gluwa supports. Get the list of banks by country, or search for a specific institution.

#### Method

<table>
<thead>
<tr>
<th>Method</th>
<th>URL</th>
</tr>
</thead>
<tbody>
<tr>
<td>GET</td>
<td><code class="highlighter-rouge">/V4.1/Banks/{country}</code></td>
</tr>
</tbody>
</table>

#### Request Parameters
<table>
<thead>
<tr>
<th style="text-align: left">Name</th>
<th style="text-align: left">Type/Value</th>
<th style="text-align: left">Required</th>
<th style="text-align: left">Path/Query</th>
<th style="text-align: left">Description</th>
 
</tr>
</thead>
<tbody>
  
<tr>
<td style="text-align: left">country</td>
<td style="text-align: left">String enum: [World, US, KR]</td>
<td style="text-align: left">yes</td>
<td style="text-align: left">Path</td>
<td style="text-align: left">Filter banks by country.</td>
</tr>
</tbody>
  
<tbody>
 
<tr>
<td style="text-align: left">offset</td>
<td style="text-align: left">Integer (int64)</td>
<td style="text-align: left">no</td>
<td style="text-align: left">Query</td>
<td style="text-align: left">Number of banks to skip from the beginning of the list; used for pagination. Defaults to 0.</td>
</tr>
    
</tbody>
 
<tbody>
  
<tr>
<td style="text-align: left">limit</td>
<td style="text-align: left">Integer (int64)</td>
<td style="text-align: left">no</td>
<td style="text-align: left">Query</td>
<td style="text-align: left">Number of banks to include in the result. Defaults to 25.</td>
</tr>
    
</tbody>
</table>

#### Responses
200

<table>
<thead>
<tr>
<th style="text-align: left">Code</th>
<th style="text-align: left">Type/Value</th>
<th style="text-align: left">Description</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: left">200</td>
<td style="text-align: left">Array: Financial Institution </td>
<td style="text-align: left">List of Banks</td>
</tr>
<tr>
<td style="text-align: left">400</td>
<td style="text-align: left">RequestValidationWithoutBodyError</td>
<td style="text-align: left">Invalid country parameter is provided.</td>
</tr>
</tbody>
</table>

#### Reponse Example

```
200
[
  {
    "InstitutionCode": "string",
    "DisplayName": "string",
    "LogoImageUrl": "string"
  }
]
```
```
400

{
  "InnerErrors": [
    {
      "Code": "string",
      "Path": "string",
      "Message": "string"
    }
  ],
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
