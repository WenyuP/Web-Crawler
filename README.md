## High level approach
We constructed a web crawler that is able to login to a website and crawl all htmls in the server and return 5 secret flags
hidden in it.

The crawler is a single-threaded crawler. We tried to implement multi-thread crawler using thread and queue.Queue,
but since we are using CPython which cannot present real multi-thread and we need to handle other issues with lock on sets and lists that will make the debug to be harder,
we decide to use only single-thread.

The program starts by login to the website, and start with a root site. We then crawl all links from that site and append it
to our queue. Our queue will check the response from the server and deal with problems when the status is 302 redirected, 
403 forbidden or 404 not found, and 500 error. If the html returned to the program contains a secret flag, we will record that flag
until 5 flags are found.

##Challenges
Creating request and cookies are two major challenges we face. For creating requests, we have to check how to handle gzip
since it throw several errors at us when the format of the request is incorrect. The solution to it is to contruct a well-formatted
request and use a helper method.

The second challenge we have is the cookie. We treat the cookie in the wrong way at first, and result in a 302 error. To 
resolve this problem, we reworked our code to solve the issue with cookie and it works fine at last.

##Testing
We used several print statement throughout the run method to check the time of the crawler spend when crawling each url, and 
the secret flags it gets when crawling. 