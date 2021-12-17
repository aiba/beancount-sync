## Background

Accounting software is useful for both individuals and businesses, in particular to keep be able to run reports on income and expenses, which leads to better financial decisions.

Most software in this category is crap, because they don't implement double-entry accounting with hierarchical accounts.  You can read all about this here:
* https://plaintextaccounting.org/
* https://beancount.github.io/docs/command_line_accounting_in_context.html#motivation
* https://beancount.github.io/docs/the_double_entry_counting_method.html#introduction

## What beancount does well

* The beancount file format seems good.
* The whole system is principled, well thought out, and well documented.
* Being able to edit in a plain text editor is good.
* Being able to script it and write custom code to generate reports is good.
* [Fava](https://github.com/beancount/fava) is good.
* The whole ecosystem is very hacker-friendly

## What beancount lacks

Beancount lacks a good, automated way to import new financial data.

The official documentation mentions [this ridiculous process](https://beancount.github.io/docs/importing_external_data.html) of manually downloading data files from every financial institution and laboriously bringing them into the beancount file.  This is too tedious.

## Alternatives to beancount

Before diving in to beancount importing, it would be nice if there were already a piece of software that did hierarchical double-entry accounting and had good import functionality, but I cannot find such software.  In particular, these apps don't work very well:

* quickbooks: the closest thing to what I want, but has fatal problems.
	* sync is good (uses OFX)
	* Implements double-entry, but limits hierarchy to 5 levels.  This is a dealbreaker.
	* UI is slow and clunky.
	* Not extensible or hacker friendly.
* gnucash
	* Excellent implementation of double-entry hierarchical accounting.
	* Spiritually very similar to beancount.
	* XML file format is not edible in a text editor.
	* Scripting is possible but annoying, requires linking into gnucash C++ codebase.
	* UI is clunky, implemented in GTK using C++.
	* Data import is so/so.  Implemented using OFX, which is not as good as plaid.  UI for reconciling is decent.
* mint.com:
	* sync is good
	* total lack of double-entry accounting
	* though sync is very good (using plaid on backend).  Also UI is slow and clunky.

## Existing efforts at beancount importing

[beancount-import](https://github.com/jbms/beancount-import).
* Fundamentally driven by importing text files that you have to download.  Downloading those text files is outside the scope of beancount-import, though there is another project, [finance-dl](https://github.com/jbms/finance-dl) that does this.  However, finance-dl only handles institutions that work with OFX, which leaves out many institutions and in general OFX data is not as good as plaid.
* Poor documentation.
* Concerning amount of open issues/bugs.
* Some potentially good ideas in the web interface.

[https://github.com/madhat2r/plaid2text](plaid2text)
* Not very actively maintained.
* I couldn't get it to work at all.
* terminal-based UI probably not as good as would be with web UI like beancount-import.
* "Mappings File" seems like a good idea.

[https://github.com/mbafford/plaid-sync/](plaid-sync)
* Haven't tried it yet.

[https://github.com/ebridges/plaid2qif](plaid2qif)
* Haven't tried it yet, but according to [this comment](https://www.reddit.com/r/plaintextaccounting/comments/qscfpm/comment/hkd1yf7/?utm_source=share&utm_medium=web2x&context=3), it works.

Basicaly [this reddit post](https://www.reddit.com/r/plaintextaccounting/comments/qktexr/ofx_imports_in_fava/) sums it up: "Importing OFX data into beancount/fava seems like FAR too much trouble, that is poorly documented, and no solution appears to work 100% correct".

## What I want

I want something like beancount-import, but which just uses plaid.  I don't mind paying for plaid API.

Requirements:
* Authenticating a bunch of institutions and selecting the accounts from those institutions which I want to sync to beancount ledger.
	* We will have to store some configuration related to this:
		* A list of institutions
		* Authentication info?
		* Mapping of plaid account to beancount account.
* Downloading all the latest financial data and writing/reconciling to beancount ledger.
	* Eventually, will want to automate the categorization, using something like plaid2text's "mappings file" or beancount-import's smart learning system.
* We probably want to store as much metadata as possible on the beancount ledger transactions, in order to avoid duplicate imports and merging transactions that get downloaded from two financial institutions.  (For example, if you transfer money from bank A to bank B, we will see this transaction twice, once for each bank.  Ultimately, it will need to become one beancount transaction in the ledger file.)
* Eventually, we might want to iterate on a UI for importing/reconciling and learning about account mappings.
	* gnucash has a great UI for this.
	* There also seem to be good ideas in beancount-import and plaid2text.


