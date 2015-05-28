# Introduction

In [php-to-excel](https://code.google.com/p/php-excel/) you can find a very simple class to convert easily a two dimensional array into a .XLS file.

I know there's a lot of classes that do the same thing, but I had no success with them. I'm going to write down how I adapted the code to make it working in CodeIgniter.

# Quick Usage

## Method 1: PHP classical way

If you don't want to use the CodeIgniter way (why and who doesn't want to use it?), you can code within your Controller:


		function file_xls()
		{
		require (dirname (__FILE__) . "/class-excel-xml.inc.php");
		$myarray =  array (        
			1 => array ("Oliver", "Peter", "Paul"),             
			array ("Marlene", "Mica", "Lina")     
		);		 	
		$xls = new Excel_XML; 		
		$xls->addArray ($myarray); 		
		$xls->generateXML ( "filename" ); 	
		}


The only thing you must do is to download the file and copy it where you like, but remember to indicate the right path where your application can find it.

## Method 2: CodeIgniter way

1. First of all, **download** the file **class-excel-xml.inc.php**. 
2. **Rename** it to **php-excel_helper.php**. 
3. **Move** it to **/application/helpers/**.

If you want to use **arrays**, you have to write within your **Controller** the following code:


		function file_xls()
		{
		$this->load->helper('php-excel');
		$data_array =  array (
		$data_array[] = array ("Oliver", "Peter", "Paul"),
				array ("Marlene", "Mica", "Lina")
				); 
		$xls = new Excel_XML;
		$xls->addArray ($data_array);
		$xls->generateXML ( "output_name" );
		}


But if you want to use the class with a MySQL database query (or whatever you wish in CodeIgniter?), this is the right, working code:


		function file_xls()
		{
		$this->load->helper('php-excel');
		$query = $this->db->get('table_name');
			foreach ($query->result() as $row)
			{
				$data_array[] = array( $row->id, $row->name, $row->surname );
			}
		$xls = new Excel_XML;
		$xls->addArray ($data_array);
		$xls->generateXML ( "output_name" );
		}

Of course, you can combine the _array method_ with the _query method_, because the only issue with the _query method_ is that only converts data, instead of field names AND data. There's no problem, because you can do it so:

   
		function file_xls()
		{
			$this->load->helper('php-excel');
			$query = $this->db->get('table_name');
			$fields = (
			$field_array[] = array ("ID", "Name", "Surname")
					);
			foreach ($query->result() as $row)
				{
				$data_array[] = array( $row->id, $row->name, $row->surname );
				}
			$xls = new Excel_XML;
			$xls->addArray ($field_array);
			$xls->addArray ($data_array);
			$xls->generateXML ( "output_name" );
		}


I hope you find it useful.

Thanks to [Oliver Schwarz](https://code.google.com/p/php-excel) for such a simple, clean and great class.

I have included the two working versions, in different folders. You can use either version 1.1 or version 2.0. In my own experience, Libreoffice deals better with version 1.1

Cheers.
