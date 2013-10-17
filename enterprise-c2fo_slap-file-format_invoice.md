## SLAP (Summary List of Accounts Payable) CSV files

* Version: 1.1.3
* Last updated June 9, 2013

###Introduction

In just a few steps C2FO customers can marshal a set of approved invoices as a text file and upload it for presentation on the C2FO Open Market.  This document helps you do this by providing:

1. A quick explanation of the **SLAP file format**
1. Detailed listing of its **Required fields**
2. Detailed listing of its **Optional fields**
3. Description of our **Secure data transfer options**

---
---

#SLAP file format

SLAP files are built using the comma-separated value (CSV) format and require a header line.  Please consult RFC4180 for help conforming to the basics and employ whichever programming language, export utility, or file-generation tools work best in your environment to automatically marshal the files themselves:

     http://tools.ietf.org/html/rfc4180
     
The kinds of data we require or make optional inside the CSV file is what defines our Summary List of Accounts Payable (**SLAP**) file format, described in the remainder of this document. 

####Definitions

* `row`: Any line in SLAP file, separated by line break.
* `header row`: Always required first row of SLAP file.  Lists field names contained in SLAP file, separated by commas.
* `field name`: human-readable label given to discrete kind of SLAP data, appears in `header row`.
	* **Required field names**: Must appear in any/every SLAP file.
	* **Optional field names**: May but does not need to appear in a SLAP file.
* `field value`: Any piece of data in a `row`, separated by one or more commas

	
####Things to note

* All field names are _case-insensitive_.
* Field names can appear in the header _in any order_.
* Values with commas must be _double-quoted_ e.g., `"My, value","My, other, value"`
* Values with apostrophe must be _double-quoted_ e.g., `"O'Doyle"`  
* Unrecognized field names will be ignored along with related data
* Field lengths are variable with a maximum of 255 characters

---
---

# Required fields

amount
---

Transaction amount, expected to be a decimal up to 2 places. More decimal places will be processed, but note that more than 2 places will be rounded up. 

Can be negative if `transaction_type`=2 is indicated.  A base invoice `transaction_type`=1 with a negative number is rejected.

currency
---

Currency code as per ISO 4217. Any currencies other than 'USD' are not accepted unless specified during the implementation process. If the value in this field is blank, it will default to 'USD'.

house_bank_code 
---

_This one only applies to customers using SAP_.

If you are an SAP customer then this is a required field.  Non-SAP customers, please disregard.

division_id
---

Indicates the organization division. If you do not have separate vendors, please disregard. 

Default: none (No Grouping)

payment_due_date
---

The due date for payment.

The date provided will be verified for legitimate date value and format.

Date formats accepted are ISO 8601(YYYY-MM-DD) or US Format(MM/DD/YYYY). Years must be 4 digits and months must be numeric and not names. Dates will be validated for valid day of month, year must be within a 10 year range of current date. If a time is included it must follow the date in 24 hour format(HH:MM:SS).


transaction_date
---

The date you record in your Accounts Payable system, typically the Invoice Date or the date you received the invoice. 

The date provided will be verified for legitimate date value and format.

Date formats accepted are ISO 8601(YYYY-MM-DD) or US Format(MM/DD/YYYY). Years must be 4 digits and months must be numeric and not names. Dates will be validated for valid day of month, year must be within a 10 year range of current date. If a time is included it must follow the date in 24 hour format(HH:MM:SS).


transaction_type
---

* 1= base transaction (amount is always positive)
* 2= adjustment (amount can be positive or negative) Adjustment can be associated to a base invoice if desired, by `adj_invoice_id`


company_id
---
Your unique ID for this vendor. Taken literally.  Leading zeroes etc preserved exactly as given.  Only exception here is leading or trailing spaces which will be clipped.  For example `00123` will remain `00123` but for spaces `' JohnCo'` will become `'JohnCo'` while `'John Co'` will remain exactly as given. 

Must be unique to this vendor across all your vendors _unless_ you are also sending `group_id` in which case each `vendor_id` must be unique within that `group_id`.


invoice_id
---

Your Transaction ID for this invoice or invoice-related record.  Must be unique in the context of company_id. The composite of invoice_id + company_id must be unique.

Case-sensitive so `INV1001` will be considered different than `inv1001` or even `Inv1001`.

Leading zeroes etc preserved exactly as given.  Only exception here, like `vendor_id`, is leading or trailing spaces.

---
---
# Optional fields

adj_invoice_id
---

Typically used to net a matched base invoice `transaction_type`=1 with an adjustment `transaction_type`=2.

If you would like to match an adjustment to a particular invoice, `adj_invoice_id` should contain the invoice_id of the invoice the adjustment should net against, i.e.:

     (invoice line, transaction_type=1) invoice_id = 123456
     (adjustment line, transaction_type=2) adj_invoice_id = 123456
     


description
---

Information housed in the Accounts Payable system, free-formt text.  Typically something like "invoice", "adjustment", or "EDI".

transaction_status
---

Information housed in the Accounts Payable system, free-form text.

voucher_id
---

Information housed in the Accounts Payable system, free-form text.  

# Custom Fields  

Any other columns sent, that are not covered in the definitions above, will be stored and returned for each transaction record.  


---
---

# Secure data transfer options

## Data pipes

The C2FO platform currently supports multiple, secure connection types for the exchange of data with our 
customers:

1. sftp (ssh) with optional white-listed IP/IP range and/or optional ssh cert
1. ftp/es (ftp-ssl) with white-listed IP/IP range
1. https for some smaller transmissions under some circumstances

API endpoints are planned for release soon, these will support similarly secure methods but provide customers with a more dynamic, less "batch mode" alternative for transmitting approved invoices.

## Data encryption

In addition you may optionally encrypt your SLAP files prior to transmission over one of the secure connections and in that case please contact your implementation manager with your specific encryption requirements. A default public PGP key that you may use to encrypt data sent to us is the most recent Pollenware cert, available here:

     http://pgp.mit.edu:11371/pks/lookup?search=pollenware&op=index

We can in some cases also support 7Zip	AES-256 encryption of data, please contact your implemenation manager if this format is desired.


 