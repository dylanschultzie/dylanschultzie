folder filter
---

```
require ["include", "environment", "variables", "relational", "comparator-i;ascii-numeric", "spamtest"];
require ["envelope", "include", "fileinto", "variables"];

# Generated: Do not run this script on spam messages
if allof (environment :matches "vnd.proton.spam-threshold" "*",
spamtest :value "ge" :comparator "i;ascii-numeric" "${1}")
{
    return;
}

if anyof(
    header :matches "X-Simplelogin-Envelope-To" "*+*@*",
    header :matches "X-Simplelogin-Envelope-To" "*.*@*"
) {

  set :lower "alias" "${1}";
  set :lower "plusalias" "${2}";

  if string :contains "${plusalias}" "shopping" {
    if string :contains "${alias}" "amazon" {
      if header :contains "subject" "Your return of" {
        fileinto "E-Commerce 🛒/amazon/returns";
        return;     
      } elsif header :contains "subject" "Your Amazon.com order has shipped" {
        fileinto "E-Commerce 🛒/amazon/shipped";
        return;    
	  } elsif header :contains "subject" "Your Amazon.com order of" {
          fileinto "E-Commerce 🛒/amazon/order";
          return;  
   	  } elsif header :contains "subject" "Delivered: Your Amazon.com order" {
          fileinto "E-Commerce 🛒/amazon/delivered";
          return;
      } 
	} else {
      fileinto "E-Commerce 🛒/amazon";
      return; 
    }
}
  if string :contains "${plusalias}" "education"     { fileinto "Education 🧠";     return; }
  if string :contains "${plusalias}" "entertainment" { fileinto "Entertainment 🎮"; return; }
  if anyof (
      string :contains "${plusalias}" "utilities",
      string :contains "${plusalias}" "bills"
  )    {
		fileinto "utilities/${alias}";
		return;
  }
  if string :contains "${plusalias}" "finance"       { fileinto "Finance 📈";       return; }
  if string :contains "${plusalias}" "cc"            { fileinto "Finance 📈/credit cards"; return; }
  if string :contains "${plusalias}" "loans"         { fileinto "Finance 📈/loans"; return; }
  if string :contains "${plusalias}" "insurance"     { fileinto "Finance 📈/insurance"; return; }
  if string :contains "${plusalias}" "government"    { fileinto "Government 🏛️";    return; }
  if string :contains "${plusalias}" "social"        { fileinto "Social 📱";        return; }
  if string :contains "${plusalias}" "tech"          { fileinto "Tech 🤖";          return; }
  if string :contains "${plusalias}" "travel"        { fileinto "Travel ✈️";         return; }
  if string :contains "${plusalias}" "work"          { fileinto "Work 💼";          return; }
  if string :contains "${plusalias}" "test"          { fileinto "Test 🧪";          return; }
}
```
