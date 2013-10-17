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

email_address
---
The email address of the contact for this organization/division.  

company_id
---
This is the ID for the organization this contact belongs to. This should match the unique ID that you provided for this vendor. Taken literally.  Leading zeroes etc preserved exactly as given.  Only exception here is leading or trailing spaces which will be clipped.  For example `00123` will remain `00123` but for spaces `' JohnCo'` will become `'JohnCo'` while `'John Co'` will remain exactly as given. 

---
---
# Optional fields

division_id
---
Required if this contact is for a specific division. This should match the unique ID that you provided for this division. Taken literally.  Leading zeroes etc preserved exactly as given.  Only exception here is leading or trailing spaces which will be clipped.  For example `00123` will remain `00123` but for spaces `' JohnCo'` will become `'JohnCo'` while `'John Co'` will remain exactly as given.

first_name
---

The first name of the contact.

last_name
---

The last name of the contact.

phone_number
---

The phone number of the contact.


address_1 & address_2
---

Address fields are validated to the extent possible, eg `20 Mainstreet` will become `20 Main St`.

city
---

Non-alphabetical characters will be removed.  Auto-capitalization similar to the `vendor_name` field will applied.

country
---
Non-alphabetical characters will be removed. Preferably the 2-letter ISO country abbreviation but full names are acceptable as well.

Default: US

postal_code
---
Postal or Zip Code as appropriate, non-alphanumeric characters removed. Limit 12 characters.

state
---
State, Province or Territory as appropriate. Limit 4 characters.


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


 