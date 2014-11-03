# COPYNEWS

Static Blogging application built on Usenet protocol, as a distributed, eventually consistent article store running in a docker container

/var/spool docker volume, shared amongst clusters

* Interfaced Into NNTP Database
	* Default Reader Application ala tilde.club, in perl? yes.
	* Custom Procfile Application Deployment

## Article Types
  
  1. Redirect, saved as null content article with `Location:` header

  1. Content, saved as article with Content-Type required, such as `text/html` or `text/x-markdown`
     Content-Type is used by `readerd` to process page for display in the browser

## Article Heierarchy

As an example: [Usenet Group Convention](http://en.wikipedia.org/wiki/Big_8_%28Usenet%29)

  * com.risingcode
    permanent redirect to https://www.risingcode.com/

  * com.risingcode.www
    latest article is mapped to http://www.risingcode.com/

  * com.risingcode.www.comp.lang.ruby
  * com.risingcode.www.news
  * com.risingode.www.alt

Articles can exist in multiple channels, via [Crossposting](http://en.wikipedia.org/wiki/Crossposting)

## Article REST API

	< POST /comp/lang/ruby HTTP/1.1
	< Content-Length: 4
	< Content-Type: text/x-markdown
	<
	< # Foo Bar Ruby
	
	> HTTP/1.1 201 Created
	
	< PUT /alt/binaries HTTP/1.1
	< Content-Length: 4
	< Content-Type: application/binary
	<
	< 1234
	
	> HTTP/1.1 201 Created

## Building Docker Image

The base image for this docker application is bootstrapped from a vagrant box provisioned with ansible

    vagrant up
    ansible-playbook -i ansible/copynews.inventory ansible/copynews-playbook.yml
    
The docker image uses `RUN /sbin/init` which is the upstart init daemon as its primary entrypoint

