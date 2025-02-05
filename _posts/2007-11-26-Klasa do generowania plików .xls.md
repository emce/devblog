---
title: Klasa do generowania plików .xls (Microsoft Excel)
date: 2007-11-26 09:12:54 +0100
categories: [php]
tags: [php,xls,microsoft excel,multiarray]
image:
  path: /assets/img/php.jpg
  alt: generowanie plików .xls
author: michal_cwiklinski
toc: false
---

# Klasa do generowania plików .xls (Microsoft Excel)

Po wielu poszukiwaniach prostej klasy do generowania "Exceli" znalazłem takową, i okazało się, że się sprawdza (przynajmniej mi się sprawdziła).

Klasa generuje kod XLSa na podstawie mutitablicy.

```php
class Excel
{
	 function xlsBOF() {
	    $data = pack("ssssss", 0x809, 0x8, 0x0, 0x10, 0x0, 0x0);  
	    return $data;
	}
	
	function xlsEOF() {
	    $data = pack("ss", 0x0A, 0x00);
	    return $data;
	}
	
	function xlsWriteNumber($Row, $Col, $Value) {
	    $data = pack("sssss", 0x203, 14, $Row, $Col, 0x0);
	    $data .= pack("d", $Value);
	    return $data;
	}
	
	function xlsWriteLabel($Row, $Col, $Value ) {
	    $L = strlen($Value);
	    $data = pack("ssssss", 0x204, 8 + $L, $Row, $Col, 0x0, $L);
	    $data .= $Value;
		return $data;
	}
	
	function generateXLS($array)
	{
		$data = $this->xlsBOF();
		$amount = count($array);
		for($i=0;$i<$amount;$i++)
		{
			$row = $array[$i];
			$amount_row = count($row);
			for($j=0;$j<$amount_row;$j++)
			{
				if((int)$row[$j])
				{
					$data .= $this->xlsWriteNumber($i,$j,$row[$j]);
				}
				else
				{
					$data .= $this->xlsWriteLabel($i,$j,$row[$j]);
				}
			}
		}
		$data .= $this->xlsEOF();
		    header("Pragma: public");
		    header("Expires: 0");
		    header("Cache-Control: must-revalidate, post-check=0, pre-check=0");
		    header("Content-Type: application/force-download");
		    header("Content-Type: application/octet-stream");
		    header("Content-Type: application/download");
		    header("Content-Disposition: attachment;filename=$filename.xls ");
		    header("Content-Transfer-Encoding: binary ");
		    echo $data;;
        }
}
```