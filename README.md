## Magento Optimization Guide

---------------------------------------
By [CoolBlueWeb](http://coolblueweb.com) (Updated on 11/12/13)

---------------------------------------

###Administration Settings

From Magento > Admin > System > Configuration

* Logging should be cleaned regularly or disabled completly. 
	* To clean logs regularly, set "Enable Log Cleaning" to "Yes" in Advanced > System > Log Cleaning
	* To disable logging, set "Enable Logs" to "No" in Advanced > Developer > Log Settings
* Enable Magento Caching in Magento > System > Cache Management
* Merge CSS and JS Files in System > Advanced > Developer
* Set Cookie Lifetime to 4200 in System > General > Web > Session Cookie Management
* Enable Compiler by running this command in Shell from the Magento Root

		php compiler.php compile
		

### Server Optimization

* Install [APC](http://us3.php.net/apc.installation) Caching (Example Setup Below)
	
		apc.enabled = 1
		apc.optimization  = 0
		apc.shm_segments = 1
		apc.shm_size = 768M
		apc.ttl = 48000
		apc.user_ttl  = 48000
		apc.num_files_hint = 8096
		apc.user_entries_hint = 8096
		apc.mmap_file_mask = /tmp/apc.XXXXXX
		apc.enable_cli = 1
		apc.cache_by_default  = 1
		apc.max_file_size = 10M
		apc.include_once_override = 0
	* Additionally, APC can be used in lieu of Magent Cache by adding the following to **local.xml**
			      
			<global>
            	...
               		<cache>
						<backend>apc</backend>
						<prefix>mgt_</prefix>
					</cache>
				...
        	</global>

* Install [mod_proxy_fcgi & php-fpm](http://wiki.apache.org/httpd/PHP-FPM)
* Verify that Apaches "Keep Alive" directive is set in .htaccess

		<IfModule mod_headers.c>
			Header set Connection keep-alive
		</IfModule>
* Verify that the memory_limit in php.ini is set to atleast 64M but no more than 2400M
* Install [mod_deflate](http://httpd.apache.org/docs/2.2/mod/mod_deflate.html) for GZIP Compression
* (OPTIONAL) Route Magento's Cache with tmpfs

		mount -t tmpfs -o size=256M,mode=0744 tmpfs /var/www/domain.com/var/cache/
		mount -t tmpfs -o size=64M,mode=0744 tmpfs /var/www/domain.com/var/session/
		
### MySQL Optimization

* Set **innodb_buffer_pool_size** to 2-3GB
* Set **query_cache_size** to 64M
* Set **query_cache_limit** to 2M
* Example Mysql Configuration
		
		innodb_thread_concurrency = 2 * [numberofCPUs] + 2 
		innodb_flush_log_at_trx_commit = 2 
		thread_concurrency = [number of CPUs] * 3 
		thread_cache_size = 32 
		table_cache = 1024 
		query_cache_size = 64M 
		query_cache_limit = 2M 
		join_buffer_size = 8M 
		tmp_table_size = 256M 
		key_buffer = 32M 
		innodb_autoextend_increment=512 
		max_allowed_packet = 16M 
		max_heap_table_size = 256M 
		read_buffer_size = 2M 
		read_rnd_buffer_size = 16M 
		bulk_insert_buffer_size = 64M 
		myisam_sort_buffer_size = 128M 
		myisam_max_sort_file_size = 10G 
		myisam_max_extra_sort_file_size = 10G 
		myisam_repair_threads = 1


### Third Party Magento Caching Tools

* [Warp Full Page Caching](http://www.magentocommerce.com/magento-connect/warp-full-page-caching.html)
* [Full Page Cache](http://www.magentocommerce.com/magento-connect/full-cache-9622.html)
