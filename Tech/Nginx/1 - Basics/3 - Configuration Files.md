Nginx consists of modules which are controlled by directives specified in the configuration file.

#### Directives
- **simple directive** - consists of the name and parameters separated by spaces and ends with a `;`
- **block directive** 
	- has the same syntax as a simple directive but ends with additional instructions surrounded by braces `{}`
	- if it can have other directives inside braces, it's called a *context*

Directives that are placed in the configuration file outside any contexts are considered to be in the **main** context.
```sh
	events  { }

	http { 
		server {
			location ;
		}
	} 
```

`server` block directives are like virtual hosts. They're distinguished by ports on which they listen to and server names. When Nginx receives a request, it decides which `server` processes a request, and proceeds to test the URI in the request header against the parameters of the location directives inside of the server block.

