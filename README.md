# Trojan SQL

## How to mess up other peoples databases by spiking SQL statements with spurious Unicode control characters

### Motivation

The idea came about when I saw someone on [mastodon](https://mastodon.social/@elbosso) creating a table
in PostgreSQL with animal emojis for column names and reading the [Trojan Source paper](https://arxiv.org/abs/2111.00169).

The final push to try and propose this as a presentation to be held on this years [rC3 2021 NOWHERE](https://events.ccc.de/2021/11/08/rc3-2021-nowhere/)
end event was a major incident i beheld at my day job.

I asked myself: "Is it possible to extend this kind of attack on databases using specially crafted SQL?"

### What needed to be done?

* Craft SQL statements that seem to a human reviewer than they are interpreted by the DBMS
* Those SQL statements must not trigger any syntax violation exceptions when trying to get them executed by the DBMS

### What did I do?

I checked for vulnerabilities in 

* Source code management (Git-Lab|-Hub|-ea)
* Editors (IDE)
* Database tools
* Databases

Of course, this was (and could not be) exhaustive but I took a few samples from the vast landscape of tools and application I thought 
susceptible to such an attack - and I was rewarded!

### What did I find?

Editors and IDEs have various degrees to which they make it obvious to a reviewer that there is something not quite right or even dubious
about the SQL statement in question.

The source code management solutions I reviewed had more surprises for me in stock: Github - already informed about Trojan Source by its original 
finders - had still no countermeasures in place in issue descriptions ans issue comments while gists and checked in source files triggered a warning message. I therefore
duly filed a security report but this was rejected on the grounds that this was not deemed a threat. I do think otherwise - if a github enterprise account 
of some team manager or similar ist
hacked and from within it comments are posted saying "I need you to put this SQL forward to production" I am sure, this succeeds in a few cases when I think one would be to many -
especially if the one dealing with the issue inspects the SQL in github and can see nothing wrong with it.

With Gitea the situation was even worse: Nowhere inside it there is any warning about suspicious charachters inside source code - not only for SQL but for none of
the languages analyzed in the original paper - there is a [pull request](https://github.com/go-gitea/gitea/pull/17562) from Nov. 5 but the discussion about the color and wording of the warning to display still rages on.
I would think it much more important to display the warning and debate such minor points later - but obviuosly my opinion differs from the one held by the gitea maintainers.

### Where to go from here?
There are many Ideas one could follow - here are only a few of them:

* Analysis of applications on Mac
* Management of instigated bug reports
* Trojan NoSQL perhaps?
* Other interpreted or Domain Specific Languages (DSLs)
* Urge other developers to mitigate

