# Code Overview

The crawler starts with a GET request to the login page. The "csrftoken" and "csrfmiddlewaretoken" are obtained from the response, and used in the POST request to the login page. The "sessionid" cookie is obtained from the response to this request. This cookie will then be included in all future GET requests.

The crawler starts on the main Fakebook page, at "https://proj5.3700.network/fakebook/". It sends a GET request to this page, parses the HTML for secret flags, and then parses the HTML for links. These links are then added to a list of links that the crawler should visit. The crawler goes through each link in the list, and repeats the process. Note that another list of all visited links is recorded, and links that have already been visited are not added to the list of future links the crawler should visit, so eventually, links will stop being added to the list and the crawler will terminate.

Upon sending a GET request, if the response is a "302" status code, the crawler simply retries the same request, but with the link replaced with the link given in the "Location" header. If the response is a "500" status code, the crawler simply retries the same request. If the response is a "403" or "404" status code, the crawler will simply move onto the next link in the list of links to visit.

Note that the program is initialized by running multiple (5) crawlers in parallel to maximize the amount of pages the crawlers will crawl on, to find flags as quickly as possible.

# Challenges Faced

One problem we ran into was that the crawler seemed to terminate almost immediately after starting. We realized this was because it was parsing and loading the log-out page, causing the crawler to log out and fail any future GET requests. We fixed this by simply intializing the list of visited pages with the log-out page, so it never parses the log out page.

# Testing Process

There are various helper functions that were used to support the larger functionality of the crawler, such as a function for sending and receiving a request, parsing a page for flags, parsing a page for new links, obtaining the initial "sessionid" cookie, etc. These functions were tested individually with specialized inputs to ensure they work correctly, and the larger functionality of the crawler was tested by observing the responses to the HTTP requests sent to Fakebook, and determining whether or not they were responding and traversing the website as expected, and using this information to determine how exactly the code may be misbehaving, and using that to make the appropriate fixes.
