---
layout: category
title: PHPExcel 
---

Download Link : `https://codeplexarchive.blob.core.windows.net/archive/projects/PHPExcel/PHPExcel.zip`

2018-03-07
코드이그나이터에 엑셀 다운로드 및 업로드를 요청받음.

Setting
```md
다운받은 PHPExcel.zip을 압축을 해제한 후 CI폴더에 부분적으로 복사한다. 
libraries 폴더에 
PHPExecel 폴더 / PHPExcel.php를 복사.
```

Load
```md
libraries 폴더에 복사했기 때문에 CI의 방법으로 로드.
$this->load->library("PHPExcel");
```

Upload
```md
try {

    /* PHPExcel Load */
    $this->load->library("PHPExcel");

    /* Excel버전을 선택 */
    $objReader = PHPExcel_IOFactory::createReader('Excel5');
    
    /* 여러파일 업로드를 대비 foreach로 받아옴. */
    foreach ($_FILES as $k => $v) {
    
        /* 임시로 업로드된 엑셀파일을 불러옴. */
        $objPHPExcel = $objReader->load($v['tmp_name']);
        
        /* 로드한 엑셀의 정보를 받아옴. */
        foreach ($objPHPExcel->getWorksheetIterator() as $worksheet) {
        
            /* Row 크기를 가져오기. */
            $highestRow         = $worksheet->getHighestRow(); // e.g. 10
            for ($row = 2; $row <= $highestRow; ++ $row) {
                
                /* 특정위치의 엑셀의 값을 가져옴. 0넘버는 A행을 뜻함. */
                $cell = $worksheet->getCellByColumnAndRow(0, $row);
            }
        }
    }
    $this->output->set_output(1);
} catch (Exception $e) {
    $this->output->set_output(0);
}
```
Download
```md

/* PHPExcel Load */
$this->load->library("PHPExcel");

// Create new PHPExcel object
$objPHPExcel = new PHPExcel();

// Set document properties
$objPHPExcel->getProperties()->setCreator("")
    ->setLastModifiedBy("")
    ->setTitle("")
    ->setSubject("")
    ->setDescription("")
    ->setKeywords("")
    ->setCategory("");

/* style setting */
$objPHPExcel->getActiveSheet()->getStyle('A1:G1')->getAlignment()->setHorizontal(PHPExcel_Style_Alignment::HORIZONTAL_CENTER);
$objPHPExcel->getActiveSheet()->getColumnDimension('A')->setWidth(8);
$objPHPExcel->getActiveSheet()->getColumnDimension('B')->setWidth(10);
$objPHPExcel->getActiveSheet()->getStyle('A:B')->getAlignment()->setHorizontal(PHPExcel_Style_Alignment::HORIZONTAL_CENTER);
$objPHPExcel->getActiveSheet()->getColumnDimension('C')->setWidth(18);
$objPHPExcel->getActiveSheet()->getStyle('C')->getAlignment()->setHorizontal(PHPExcel_Style_Alignment::HORIZONTAL_CENTER);
$objPHPExcel->getActiveSheet()->getColumnDimension('D')->setWidth(35);
$objPHPExcel->getActiveSheet()->getColumnDimension('E')->setWidth(15);
$objPHPExcel->getActiveSheet()->getColumnDimension('F')->setWidth(20);
$objPHPExcel->getActiveSheet()->getColumnDimension('G')->setWidth(15);
$objPHPExcel->getActiveSheet()->getStyle('G')->getAlignment()->setHorizontal(PHPExcel_Style_Alignment::HORIZONTAL_CENTER);

// Add some data
$objPHPExcel->setActiveSheetIndex(0)
    ->setCellValue('A1', '#')
    ->setCellValue('B1', '수령인')
    ->setCellValue('C1', '가상번호')
    ->setCellValue('D1', '수령주소')
    ->setCellValue('E1', '택배회사')
    ->setCellValue('F1', '송장번호')
    ->setCellValue('G1', '문자발송여부');

/* 첫번째줄은 다른값이 있어 두번째줄부터 시작 */
$num = 2;
foreach ($arr as $k => $v) {

    /* 붉은 테두리 */
    $objPHPExcel->getActiveSheet()->getStyle('E'.$num)->applyFromArray(
        array(
            'borders' => array(
                'allborders' => array(
                    'style' => PHPExcel_Style_Border::BORDER_THIN,
                    'color' => array('rgb' => 'ff0000')
                )
            )
        )
    );
    $objPHPExcel->getActiveSheet()->getStyle('F'.$num)->applyFromArray(
        array(
            'borders' => array(
                'allborders' => array(
                    'style' => PHPExcel_Style_Border::BORDER_THIN,
                    'color' => array('rgb' => 'ff0000')
                )
            )
        )
    );
    $objPHPExcel->setActiveSheetIndex(0)
        ->setCellValueExplicit('A'.$num, $v, PHPExcel_Cell_DataType::TYPE_STRING)
        ->setCellValueExplicit('B'.$num, $v, PHPExcel_Cell_DataType::TYPE_STRING)
        ->setCellValueExplicit('C'.$num, $v, PHPExcel_Cell_DataType::TYPE_STRING)
        ->setCellValueExplicit('D'.$num, $v, PHPExcel_Cell_DataType::TYPE_STRING)
        ->setCellValueExplicit('E'.$num, $v, PHPExcel_Cell_DataType::TYPE_STRING)
        ->setCellValueExplicit('F'.$num, $v, PHPExcel_Cell_DataType::TYPE_STRING)
        ->setCellValueExplicit('G'.$num, $v, PHPExcel_Cell_DataType::TYPE_STRING);

    /* 다음줄로 이동.  */
    $num++;
}

// Rename worksheet
$objPHPExcel->getActiveSheet()->setTitle($excelTitle);

// Set active sheet index to the first sheet, so Excel opens this as the first sheet
$objPHPExcel->setActiveSheetIndex(0);

/* 익스플로러에서 UTF-8로 한글깨짐현상이 발생. */
$excelTitle = iconv('UTF-8', 'EUC-KR', $excelTitle);
// Redirect output to a client’s web browser (Excel5)
header('Content-Type: application/vnd.ms-excel');
header('Content-Disposition: attachment;filename="'.date('Y-m-d ').$excelTitle.'.xls"');
header('Cache-Control: max-age=0');

$objWriter = PHPExcel_IOFactory::createWriter($objPHPExcel, 'Excel5');
$objWriter->save('php://output');
exit;
```