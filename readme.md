<html>

<body lang=RU link="#0563C1" vlink="#954F72">

<div class=WordSection1>

<h3><p class=MsoNormal>Разработка шаблона для отчета, который принимает на вход
несколько параметров и файл с разметкой и, используя данные о кассах и продажах
в базе SQLite, формирует файл с отчетом.</p></h3>

<p class=MsoNormal style='margin-top:24.0pt;margin-right:0cm;margin-bottom:
0cm;margin-left:21.6pt;margin-bottom:.0001pt;text-indent:-21.6pt;page-break-after:
avoid;border:none'><b><span style='font-size:14.0pt;line-height:115%;
color:#2E75B5'>1<span style='font:7.0pt "Times New Roman"'>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</span></span></b><b><span style='font-size:14.0pt;line-height:115%;color:#2E75B5'>Исходные
данные</span></b></p>

<p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt'>Исходные
данные включают:</p>

<p class=MsoNormal style='margin-top:0cm;margin-right:0cm;margin-bottom:0cm;
margin-left:36.0pt;margin-bottom:.0001pt;text-indent:-18.0pt;border:none'><span
style='font-family:"Noto Sans Symbols"'>&#9679;<span style='font:7.0pt "Times New Roman"'>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</span></span><span style='color:black'>Файл с разметкой двух брендов в формате
csv с полями:</span></p>

<p class=MsoNormal style='margin-top:0cm;margin-right:0cm;margin-bottom:0cm;
margin-left:72.0pt;margin-bottom:.0001pt;text-indent:-18.0pt;border:none'><span
style='font-family:"Courier New"'>o<span style='font:7.0pt "Times New Roman"'>&nbsp;&nbsp;
</span></span><span style='color:black'>brand</span></p>

<p class=MsoNormal style='margin-top:0cm;margin-right:0cm;margin-bottom:0cm;
margin-left:72.0pt;margin-bottom:.0001pt;text-indent:-18.0pt;border:none'><span
style='font-family:"Courier New"'>o<span style='font:7.0pt "Times New Roman"'>&nbsp;&nbsp;
</span></span><span style='color:black'>product_name_hash (хэш от названия
товара, по которому можно найти интересующие продажи в чеках)</span></p>

<p class=MsoNormal style='margin-top:0cm;margin-right:0cm;margin-bottom:0cm;
margin-left:36.0pt;margin-bottom:.0001pt;text-indent:-18.0pt;border:none'><span
style='font-family:"Noto Sans Symbols"'>&#9679;<span style='font:7.0pt "Times New Roman"'>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</span></span><span style='color:black'>База данных SQLite (</span><a
href="https://www.sqlite.org/"><span style='color:#0563C1'>https://www.sqlite.org/</span></a><span
style='color:black'>) со следующими таблицами:</span></p>

<p class=MsoNormal style='margin-top:0cm;margin-right:0cm;margin-bottom:0cm;
margin-left:72.0pt;margin-bottom:.0001pt;text-indent:-18.0pt;border:none'><span
style='font-family:"Courier New"'>o<span style='font:7.0pt "Times New Roman"'>&nbsp;&nbsp;
</span></span><span style='color:black'>kkt_activity – данные о первом и
последнем чеке для всех касс</span></p>

<p class=MsoNormal style='margin-top:0cm;margin-right:0cm;margin-bottom:0cm;
margin-left:72.0pt;margin-bottom:.0001pt;text-indent:-18.0pt;border:none'><span
style='font-family:"Courier New"'>o<span style='font:7.0pt "Times New Roman"'>&nbsp;&nbsp;
</span></span><span style='color:black'>kkt_categories – категории касс по виду
деятельности</span></p>

<p class=MsoNormal style='margin-top:0cm;margin-right:0cm;margin-bottom:0cm;
margin-left:72.0pt;margin-bottom:.0001pt;text-indent:-18.0pt;border:none'><span
style='font-family:"Courier New"'>o<span style='font:7.0pt "Times New Roman"'>&nbsp;&nbsp;
</span></span><span style='color:black'>kkt_info – основная информация о
кассах, включая данные организации и торговой точки</span></p>

<p class=MsoNormal style='margin-top:0cm;margin-right:0cm;margin-bottom:0cm;
margin-left:72.0pt;margin-bottom:.0001pt;text-indent:-18.0pt;border:none'><span
style='font-family:"Courier New"'>o<span style='font:7.0pt "Times New Roman"'>&nbsp;&nbsp;
</span></span><span style='color:black'>sales – продажи касс (чеки)</span></p>

<p class=MsoNormal style='margin-left:36.0pt;border:none'><b><i><span
style='color:black'>Примечание</span></i></b><span style='color:black'>. Все
названия полей в таблицах унифицированы. Таким образом, поля с одинаковым
названием в разных таблицах ссылаются на одни и те же сущности, и по ним можно
джойнить таблицы (например, kkt_number во всех таблицах указывает на конкретную
уникальную кассу).</span></p>

<p class=MsoNormal>Ссылка на архив: <a
href="https://drive.google.com/file/d/1jtypNNuAfbmoVZXfbxhs419sSx1z6Y5X/view?usp=sharing"><span
style='color:#0563C1'>https://drive.google.com/file/d/1jtypNNuAfbmoVZXfbxhs419sSx1z6Y5X/view?usp=sharing</span></a></p>

<p class=MsoNormal style='margin-top:10.0pt;margin-right:0cm;margin-bottom:
0cm;margin-left:28.8pt;margin-bottom:.0001pt;text-indent:-28.8pt;page-break-after:
avoid;border:none'><b><span style='font-size:13.0pt;line-height:115%;
color:#5B9BD5'>1.1<span style='font:7.0pt "Times New Roman"'>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</span></span></b><b><span style='font-size:13.0pt;line-height:115%;color:#5B9BD5'>Таблица
kkt_activity</span></b></p>

<p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt'>Описание
полей:</p>

<table class=a9 border=1 cellspacing=0 cellpadding=0 width=649
 style='border-collapse:collapse;border:none'>
 <tr>
  <td width=245 valign=top style='width:183.7pt;border:solid #BDD7EE 1.0pt;
  border-bottom:solid #8EAADB 1.5pt;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>Поле</p>
  </td>
  <td width=404 valign=top style='width:303.1pt;border-top:solid #BDD7EE 1.0pt;
  border-left:none;border-bottom:solid #8EAADB 1.5pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>Описание</p>
  </td>
 </tr>
 <tr>
  <td width=245 valign=top style='width:183.7pt;border:solid #BDD7EE 1.0pt;
  border-top:none;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>kkt_number</p>
  </td>
  <td width=404 valign=top style='width:303.1pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>Регистрационный номер кассы</p>
  </td>
 </tr>
 <tr>
  <td width=245 valign=top style='width:183.7pt;border:solid #BDD7EE 1.0pt;
  border-top:none;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>receipt_date_min</p>
  </td>
  <td width=404 valign=top style='width:303.1pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>Дата первого чека, полученного от кассы</p>
  </td>
 </tr>
 <tr>
  <td width=245 valign=top style='width:183.7pt;border:solid #BDD7EE 1.0pt;
  border-top:none;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>receipt_date_max</p>
  </td>
  <td width=404 valign=top style='width:303.1pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>Дата последнего чека, полученного от кассы</p>
  </td>
 </tr>
</table>

<p class=MsoNormal style='margin-top:10.0pt;margin-right:0cm;margin-bottom:
0cm;margin-left:28.8pt;margin-bottom:.0001pt;text-indent:-28.8pt;page-break-after:
avoid;border:none'><b><span style='font-size:13.0pt;line-height:115%;
color:#5B9BD5'>1.2<span style='font:7.0pt "Times New Roman"'>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</span></span></b><b><span style='font-size:13.0pt;line-height:115%;color:#5B9BD5'>Таблица
kkt_categories</span></b></p>

<p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt'>Описание
полей:</p>

<table class=aa border=1 cellspacing=0 cellpadding=0 width=649
 style='border-collapse:collapse;border:none'>
 <tr>
  <td width=245 valign=top style='width:183.7pt;border:solid #BDD7EE 1.0pt;
  border-bottom:solid #8EAADB 1.5pt;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>Поле</p>
  </td>
  <td width=404 valign=top style='width:303.1pt;border-top:solid #BDD7EE 1.0pt;
  border-left:none;border-bottom:solid #8EAADB 1.5pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>Описание</p>
  </td>
 </tr>
 <tr>
  <td width=245 valign=top style='width:183.7pt;border:solid #BDD7EE 1.0pt;
  border-top:none;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>kkt_number</p>
  </td>
  <td width=404 valign=top style='width:303.1pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>Регистрационный номер кассы</p>
  </td>
 </tr>
 <tr>
  <td width=245 valign=top style='width:183.7pt;border:solid #BDD7EE 1.0pt;
  border-top:none;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>category</p>
  </td>
  <td width=404 valign=top style='width:303.1pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>Категория кассы по виду деятельности</p>
  </td>
 </tr>
 <tr>
  <td width=245 valign=top style='width:183.7pt;border:solid #BDD7EE 1.0pt;
  border-top:none;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>date_from</p>
  </td>
  <td width=404 valign=top style='width:303.1pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>Дата начала действия записи (включается)</p>
  </td>
 </tr>
 <tr>
  <td width=245 valign=top style='width:183.7pt;border:solid #BDD7EE 1.0pt;
  border-top:none;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>date_till</p>
  </td>
  <td width=404 valign=top style='width:303.1pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>Дата окончания действия записи (не включается)</p>
  </td>
 </tr>
 <tr>
  <td width=245 valign=top style='width:183.7pt;border:solid #BDD7EE 1.0pt;
  border-top:none;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>version</p>
  </td>
  <td width=404 valign=top style='width:303.1pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>Версия классификации</p>
  </td>
 </tr>
</table>

<p class=MsoNormal style='margin-top:12.0pt;margin-right:0cm;margin-bottom:
0cm;margin-left:0cm;margin-bottom:.0001pt;page-break-after:avoid'>Примечания к
таблице:</p>

<ol style='margin-top:0cm' start=1 type=1>
 <li class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;border:
     none'><span style='color:black'>Таблица содержит несколько версий
     классификации. Когда появляется новая версия классификатора торговых
     точек, то все кассы классифицируются за все время их работы и в таблице
     появляются новые записи для новой версии, при этом старые версии не
     удаляются и могут использоваться в старых отчетах.</span></li>
 <li class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;border:
     none'><span style='color:black'>Классификация касс производится каждую
     неделю (так как касса могла начать торговать другими товарами). Таким
     образом, каждую неделю для каждой работающей кассы появляется новая запись
     в таблице для каждой версии классификатора.</span></li>
 <li class=MsoNormal style='border:none'><span style='color:black'>В таблице
     хранятся только записи для работающих касс, которые удалось
     классифицировать. Остальных касс в таблице нет.</span></li>
</ol>

<p class=MsoNormal style='margin-top:10.0pt;margin-right:0cm;margin-bottom:
0cm;margin-left:28.8pt;margin-bottom:.0001pt;text-indent:-28.8pt;page-break-after:
avoid;border:none'><b><span style='font-size:13.0pt;line-height:115%;
color:#5B9BD5'>1.3<span style='font:7.0pt "Times New Roman"'>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</span></span></b><b><span style='font-size:13.0pt;line-height:115%;color:#5B9BD5'>Таблица
kkt_info</span></b></p>

<p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt'>Описание
полей:</p>

<table class=ab border=1 cellspacing=0 cellpadding=0 width=649
 style='border-collapse:collapse;border:none'>
 <tr>
  <td width=245 valign=top style='width:183.7pt;border:solid #BDD7EE 1.0pt;
  border-bottom:solid #8EAADB 1.5pt;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>Поле</p>
  </td>
  <td width=404 valign=top style='width:303.1pt;border-top:solid #BDD7EE 1.0pt;
  border-left:none;border-bottom:solid #8EAADB 1.5pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>Описание</p>
  </td>
 </tr>
 <tr>
  <td width=245 valign=top style='width:183.7pt;border:solid #BDD7EE 1.0pt;
  border-top:none;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>org_inn</p>
  </td>
  <td width=404 valign=top style='width:303.1pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>ИНН организации</p>
  </td>
 </tr>
 <tr>
  <td width=245 valign=top style='width:183.7pt;border:solid #BDD7EE 1.0pt;
  border-top:none;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>shop_id</p>
  </td>
  <td width=404 valign=top style='width:303.1pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>Идентификатор торговой точки (уникален в рамках конкретной
  организации)</p>
  </td>
 </tr>
 <tr>
  <td width=245 valign=top style='width:183.7pt;border:solid #BDD7EE 1.0pt;
  border-top:none;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>kkt_number</p>
  </td>
  <td width=404 valign=top style='width:303.1pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>Регистрационный номер кассы</p>
  </td>
 </tr>
 <tr style='height:12.75pt'>
  <td width=245 valign=top style='width:183.7pt;border:solid #BDD7EE 1.0pt;
  border-top:none;padding:0cm 5.4pt 0cm 5.4pt;height:12.75pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>region</p>
  </td>
  <td width=404 valign=top style='width:303.1pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt;height:12.75pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>Регион, в котором работает касса</p>
  </td>
 </tr>
 <tr>
  <td width=245 valign=top style='width:183.7pt;border:solid #BDD7EE 1.0pt;
  border-top:none;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>date_from</p>
  </td>
  <td width=404 valign=top style='width:303.1pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>Дата начала действия записи (включается)</p>
  </td>
 </tr>
 <tr>
  <td width=245 valign=top style='width:183.7pt;border:solid #BDD7EE 1.0pt;
  border-top:none;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>date_till</p>
  </td>
  <td width=404 valign=top style='width:303.1pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>Дата окончания действия записи (не включается)</p>
  </td>
 </tr>
</table>

<p class=MsoNormal style='margin-top:12.0pt;margin-right:0cm;margin-bottom:
0cm;margin-left:0cm;margin-bottom:.0001pt'>Примечания к таблице:</p>

<ol style='margin-top:0cm' start=1 type=1>
 <li class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;border:
     none'><span style='color:black'>Таблица содержит основную информацию о
     кассе за всю историю ее работы.</span></li>
 <li class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;border:
     none'><span style='color:black'>Для каждой кассы в таблице может быть
     несколько записей, которые не пересекаются по периоду действия,
     обозначенному датами date_from и date_till.</span></li>
 <li class=MsoNormal style='border:none'><span style='color:black'>Данные
     таблицы являются выжимкой из более полной таблицы, поэтому в ней могут
     быть полностью идентичные записи с разными периодом действия. Это значит,
     что в исходной таблице изменились какие-то параметры кассы, которые в итоге
     не попали в выжимку для текущего задания. Это никак не влияет на принципы
     работы с таблицей.</span></li>
</ol>

<p class=MsoNormal style='margin-top:10.0pt;margin-right:0cm;margin-bottom:
0cm;margin-left:28.8pt;margin-bottom:.0001pt;text-indent:-28.8pt;page-break-after:
avoid;border:none'><b><span style='font-size:13.0pt;line-height:115%;
color:#5B9BD5'>1.4<span style='font:7.0pt "Times New Roman"'>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</span></span></b><b><span style='font-size:13.0pt;line-height:115%;color:#5B9BD5'>Таблица
sales</span></b></p>

<p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt'>Описание
полей:</p>

<table class=ac border=1 cellspacing=0 cellpadding=0 width=649
 style='border-collapse:collapse;border:none'>
 <tr>
  <td width=245 valign=top style='width:183.7pt;border:solid #BDD7EE 1.0pt;
  border-bottom:solid #8EAADB 1.5pt;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>Поле</p>
  </td>
  <td width=404 valign=top style='width:303.1pt;border-top:solid #BDD7EE 1.0pt;
  border-left:none;border-bottom:solid #8EAADB 1.5pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>Описание</p>
  </td>
 </tr>
 <tr>
  <td width=245 valign=top style='width:183.7pt;border:solid #BDD7EE 1.0pt;
  border-top:none;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>org_inn</p>
  </td>
  <td width=404 valign=top style='width:303.1pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>ИНН организации</p>
  </td>
 </tr>
 <tr>
  <td width=245 valign=top style='width:183.7pt;border:solid #BDD7EE 1.0pt;
  border-top:none;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>kkt_number</p>
  </td>
  <td width=404 valign=top style='width:303.1pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>Регистрационный номер кассы</p>
  </td>
 </tr>
 <tr>
  <td width=245 valign=top style='width:183.7pt;border:solid #BDD7EE 1.0pt;
  border-top:none;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>receipt_date</p>
  </td>
  <td width=404 valign=top style='width:303.1pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>Дата продажи (чека)</p>
  </td>
 </tr>
 <tr>
  <td width=245 valign=top style='width:183.7pt;border:solid #BDD7EE 1.0pt;
  border-top:none;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>receipt_id</p>
  </td>
  <td width=404 valign=top style='width:303.1pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>Идентификатор продажи (чека)</p>
  </td>
 </tr>
 <tr>
  <td width=245 valign=top style='width:183.7pt;border:solid #BDD7EE 1.0pt;
  border-top:none;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>product_name_hash</p>
  </td>
  <td width=404 valign=top style='width:303.1pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>Хэш от названия товара</p>
  </td>
 </tr>
 <tr>
  <td width=245 valign=top style='width:183.7pt;border:solid #BDD7EE 1.0pt;
  border-top:none;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>total_sum</p>
  </td>
  <td width=404 valign=top style='width:303.1pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>Сумма продажи товара</p>
  </td>
 </tr>
</table>

<p class=MsoNormal style='margin-top:24.0pt;margin-right:0cm;margin-bottom:
0cm;margin-left:21.6pt;margin-bottom:.0001pt;text-indent:-21.6pt;page-break-after:
avoid;border:none'><b><span style='font-size:14.0pt;line-height:115%;
color:#2E75B5'>2<span style='font:7.0pt "Times New Roman"'>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</span></span></b><b><span style='font-size:14.0pt;line-height:115%;color:#2E75B5'>Требования
к отчету</span></b></p>

<p class=MsoNormal style='margin-top:10.0pt;margin-right:0cm;margin-bottom:
0cm;margin-left:28.9pt;margin-bottom:.0001pt;text-indent:-28.9pt;border:none'><b><span
style='font-size:13.0pt;line-height:115%;color:#5B9BD5'>2.1<span
style='font:7.0pt "Times New Roman"'>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</span></span></b><b><span style='font-size:13.0pt;line-height:115%;color:#5B9BD5'>Входящие
параметры</span></b></p>

<p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt'>Должна быть
возможность задать следующие параметры отчета:</p>

<ol style='margin-top:0cm' start=1 type=1>
 <li class=MsoNormal style='color:black;margin-bottom:0cm;margin-bottom:.0001pt;
     border:none'>Путь к файлу<span lang=EN-US> product_names.csv.</span></li>
 <li class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;border:
     none'><span style='color:black'>Период, задается двумя датами в формате
     yyyy-MM-dd (обе даты включаются в отчет):</span></li>
 <ol style='margin-top:0cm' start=1 type=a>
  <li class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;
      border:none'><span style='color:black'>date_from</span></li>
  <li class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;
      border:none'><span style='color:black'>date_to</span></li>
 </ol>
 <li class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;border:
     none'><span style='color:black'>Фильтр kkt_category, принимающий на вход
     список категорий: может быть пустым или содержать любое кол-во категорий
     (например: FMCG, HoReCa).</span></li>
 <li class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;border:
     none'><span style='color:black'>Признак необходимости группировки данных
     отчета по следующим полям:</span></li>
 <ol style='margin-top:0cm' start=1 type=a>
  <li class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;
      border:none'><span style='color:black'>receipt_date</span></li>
  <li class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;
      border:none'><span style='color:black'>region</span></li>
  <li class=MsoNormal style='border:none'><span style='color:black'>channel
      (данное поле вычисляется, описание ниже)</span></li>
 </ol>
</ol>

<p class=MsoNormal style='margin-top:10.0pt;margin-right:0cm;margin-bottom:
0cm;margin-left:28.8pt;margin-bottom:.0001pt;text-indent:-28.8pt;page-break-after:
avoid;border:none'><b><span style='font-size:13.0pt;line-height:115%;
color:#5B9BD5'>2.2<span style='font:7.0pt "Times New Roman"'>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</span></span></b><b><span style='font-size:13.0pt;line-height:115%;color:#5B9BD5'>Содержание
отчета</span></b></p>

<p class=MsoNormal>Отчет всегда формируется по продажам брендов из входящего
файла product_names.csv в соответствии с разметкой. То есть для каждого бренда
(brand) из файла нужно найти все продажи, имеющие соответствующие названия
товаров (product_name_hash). Все прочие продажи в отчет попадать не должны.</p>

<p class=MsoNormal style='margin-top:10.0pt;margin-right:0cm;margin-bottom:
0cm;margin-left:36.0pt;margin-bottom:.0001pt;text-indent:-36.0pt;page-break-after:
avoid;border:none'><b><span style='color:#5B9BD5'>2.2.1<span style='font:7.0pt "Times New Roman"'>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</span></span></b><b><span style='color:#5B9BD5'>Фильтры</span></b></p>

<p class=MsoNormal>У отчета должен быть только один опциональный фильтр
kkt_category. Если он пустой, то параметр игнорируется. В противном случае при
формировании отчета должны учитываться только продажи касс указанных категорий.
В данном отчете используется всегда наиболее актуальная версия классификации
(максимальная дата в поле version).</p>

<p class=MsoNormal style='margin-top:10.0pt;margin-right:0cm;margin-bottom:
0cm;margin-left:36.0pt;margin-bottom:.0001pt;text-indent:-36.0pt;page-break-after:
avoid;border:none'><b><span style='color:#5B9BD5'>2.2.2<span style='font:7.0pt "Times New Roman"'>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</span></span></b><b><span style='color:#5B9BD5'>Разрезы</span></b></p>

<p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt'>У отчета
есть один обязательные разрез, который должен быть всегда – brand. Остальные
разрезы опциональны и зависят от указанных признаков необходимости группировки
во входящих параметрах отчета:</p>

<p class=MsoNormal style='margin-top:0cm;margin-right:0cm;margin-bottom:0cm;
margin-left:36.0pt;margin-bottom:.0001pt;text-indent:-18.0pt;border:none'><span
style='font-family:"Noto Sans Symbols"'>&#9679;<span style='font:7.0pt "Times New Roman"'>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</span></span><span style='color:black'>receipt_date: если признак указан, то
отчет должен быть дополнительно сгруппирован по полю receipt_date из таблицы
sales</span></p>

<p class=MsoNormal style='margin-top:0cm;margin-right:0cm;margin-bottom:0cm;
margin-left:36.0pt;margin-bottom:.0001pt;text-indent:-18.0pt;border:none'><span
style='font-family:"Noto Sans Symbols"'>&#9679;<span style='font:7.0pt "Times New Roman"'>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</span></span><span style='color:black'>region: если признак указан, то отчет
должен быть дополнительно сгруппирован по полю region из таблицы kkt_info</span></p>

<p class=MsoNormal style='margin-left:36.0pt;text-indent:-18.0pt;border:none'><span
style='font-family:"Noto Sans Symbols"'>&#9679;<span style='font:7.0pt "Times New Roman"'>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</span></span><span style='color:black'>channel: если признак указан, то отчет
должен быть дополнительно сгруппирован по полю channel, которое необходимо
вычислить</span></p>

<p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt'>Значение
поля channel указывает на то, является ли организация торговой сетью, и
вычисляется в соответствии со следующими правилами:</p>

<p class=MsoNormal style='margin-top:0cm;margin-right:0cm;margin-bottom:0cm;
margin-left:38.5pt;margin-bottom:.0001pt;text-indent:-18.0pt;border:none'>1.<span
style='font:7.0pt "Times New Roman"'>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span><span
style='color:black'>Значение вычисляется для организации целиком, то есть
привязано к уникальному значению org_inn.</span></p>

<p class=MsoNormal style='margin-top:0cm;margin-right:0cm;margin-bottom:0cm;
margin-left:38.5pt;margin-bottom:.0001pt;text-indent:-18.0pt;border:none'>2.<span
style='font:7.0pt "Times New Roman"'>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span><span
style='color:black'>Значения зависят от количества активных торговых точек
(shop_id из таблицы kkt_info в периоде, за который строится отчет (входящие
параметры date_from и date_to).</span></p>

<p class=MsoNormal style='margin-top:0cm;margin-right:0cm;margin-bottom:0cm;
margin-left:38.5pt;margin-bottom:.0001pt;text-indent:-18.0pt;border:none'>3.<span
style='font:7.0pt "Times New Roman"'>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span><span
style='color:black'>Торговая точка считается активной, если есть хотя бы одна
касса, у которой период активности (receipt_date_min и receipt_date_max из
таблицы kkt_activity) пересекается с периодом отчета.</span></p>

<p class=MsoNormal style='margin-top:0cm;margin-right:0cm;margin-bottom:0cm;
margin-left:38.5pt;margin-bottom:.0001pt;text-indent:-18.0pt;border:none'>4.<span
style='font:7.0pt "Times New Roman"'>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span><span
style='color:black'>Возможные значения:</span></p>

<p class=MsoNormal style='margin-top:0cm;margin-right:0cm;margin-bottom:0cm;
margin-left:74.5pt;margin-bottom:.0001pt;text-indent:-18.0pt;border:none'>a.<span
style='font:7.0pt "Times New Roman"'>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span><span
style='color:black'>nonchain: у организации &lt; 3 активных торговых точек в
периоде, за который формируется отчет</span></p>

<p class=MsoNormal style='margin-left:74.5pt;text-indent:-18.0pt;border:none'>b.<span
style='font:7.0pt "Times New Roman"'>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span><span
style='color:black'>chain: у организации &gt;= 3 активных торговых точек в
периоде, за который формируется отчет</span></p>

<p class=MsoNormal style='margin-top:10.0pt;margin-right:0cm;margin-bottom:
0cm;margin-left:36.0pt;margin-bottom:.0001pt;text-indent:-36.0pt;page-break-after:
avoid;border:none'><b><span style='color:#5B9BD5'>2.2.3<span style='font:7.0pt "Times New Roman"'>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</span></span></b><b><span style='color:#5B9BD5'>Показатели</span></b></p>

<p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt'>В рамках
отчета рассчитываются два показателя:</p>

<ol style='margin-top:0cm' start=1 type=1>
 <li class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;border:
     none'><span style='color:black'>total_sum – сумма продаж бренда (по полю
     total_sum из таблицы sales)</span></li>
 <li class=MsoNormal style='border:none'><span style='color:black'>total_sum_pct
     – процент продаж бренда от продаж всех брендов (total_sum бренда,
     разделенный на сумму total_sum всех брендов в отчете, округленный до 2-х
     знаков после запятой)</span></li>
</ol>

<p class=MsoNormal style='margin-top:10.0pt;margin-right:0cm;margin-bottom:
0cm;margin-left:36.0pt;margin-bottom:.0001pt;text-indent:-36.0pt;page-break-after:
avoid;border:none'><b><span style='color:#5B9BD5'>2.2.4<span style='font:7.0pt "Times New Roman"'>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</span></span></b><b><span style='color:#5B9BD5'>Формат</span></b></p>

<p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt'>Отчет должен
формироваться в виде файла:</p>

<p class=MsoNormal style='margin-top:0cm;margin-right:0cm;margin-bottom:0cm;
margin-left:36.0pt;margin-bottom:.0001pt;text-indent:-18.0pt;border:none'><span
style='font-family:"Noto Sans Symbols"'>&#9679;<span style='font:7.0pt "Times New Roman"'>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</span></span><span style='color:black'>Название: report</span></p>

<p class=MsoNormal style='margin-top:0cm;margin-right:0cm;margin-bottom:0cm;
margin-left:36.0pt;margin-bottom:.0001pt;text-indent:-18.0pt;border:none'><span
style='font-family:"Noto Sans Symbols"'>&#9679;<span style='font:7.0pt "Times New Roman"'>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</span></span><span style='color:black'>Формат: csv</span></p>

<p class=MsoNormal style='margin-top:0cm;margin-right:0cm;margin-bottom:0cm;
margin-left:36.0pt;margin-bottom:.0001pt;text-indent:-18.0pt;border:none'><span
style='font-family:"Noto Sans Symbols"'>&#9679;<span style='font:7.0pt "Times New Roman"'>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</span></span><span style='color:black'>Заголовки: названия полей</span></p>

<p class=MsoNormal style='margin-left:36.0pt;text-indent:-18.0pt;border:none'><span
style='font-family:"Noto Sans Symbols"'>&#9679;<span style='font:7.0pt "Times New Roman"'>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</span></span><span style='color:black'>Разделитель: запятая</span></p>

<p class=MsoNormal style='margin-top:10.0pt;margin-right:0cm;margin-bottom:
0cm;margin-left:28.8pt;margin-bottom:.0001pt;text-indent:-28.8pt;page-break-after:
avoid;border:none'><b><span style='font-size:13.0pt;line-height:115%;
color:#5B9BD5'>2.3<span style='font:7.0pt "Times New Roman"'>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</span></span></b><b><span style='font-size:13.0pt;line-height:115%;color:#5B9BD5'>Примеры
отчета</span></b></p>

<p class=MsoNormal>Данные в примерах не являются реальными результатами, а
приведены </p>

<p class=MsoNormal style='margin-top:10.0pt;margin-right:0cm;margin-bottom:
0cm;margin-left:36.0pt;margin-bottom:.0001pt;text-indent:-36.0pt;page-break-after:
avoid;border:none'><b><span style='color:#5B9BD5'>2.3.1<span style='font:7.0pt "Times New Roman"'>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</span></span></b><b><span style='color:#5B9BD5'>Пример №1</span></b></p>

<p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt'>Входящие
параметры:</p>

<ol style='margin-top:0cm' start=1 type=1>
 <li class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;border:
     none'><span style='color:black'>Период:</span></li>
 <ol style='margin-top:0cm' start=1 type=a>
  <li class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;
      border:none'><span style='color:black'>date_from: “2019-08-01”</span></li>
  <li class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;
      border:none'><span style='color:black'>date_to: “2019-08-02”</span></li>
 </ol>
 <li class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;border:
     none'><span style='color:black'>Фильтр kkt_category: не указан.</span></li>
 <li class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;border:
     none'><span style='color:black'>Разрезы:</span></li>
 <ol style='margin-top:0cm' start=1 type=a>
  <li class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;
      border:none'><span style='color:black'>receipt_date: false</span></li>
  <li class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;
      border:none'><span style='color:black'>region: false</span></li>
  <li class=MsoNormal style='border:none'><span style='color:black'>channel:
      false</span></li>
 </ol>
</ol>

<p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt'>Результат
(все числа взяты случайным образом для примера):</p>

<table class=ad border=1 cellspacing=0 cellpadding=0 width=649
 style='border-collapse:collapse;border:none'>
 <tr>
  <td width=216 valign=top style='width:162.25pt;border:solid #BDD7EE 1.0pt;
  border-bottom:solid #8EAADB 1.5pt;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>brand</p>
  </td>
  <td width=216 valign=top style='width:162.25pt;border-top:solid #BDD7EE 1.0pt;
  border-left:none;border-bottom:solid #8EAADB 1.5pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>total_sum</p>
  </td>
  <td width=216 valign=top style='width:162.3pt;border-top:solid #BDD7EE 1.0pt;
  border-left:none;border-bottom:solid #8EAADB 1.5pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>total_sum_pct</p>
  </td>
 </tr>
 <tr>
  <td width=216 valign=top style='width:162.25pt;border:solid #BDD7EE 1.0pt;
  border-top:none;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>parliament</p>
  </td>
  <td width=216 valign=top style='width:162.25pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>200</p>
  </td>
  <td width=216 valign=top style='width:162.3pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>0.67</p>
  </td>
 </tr>
 <tr>
  <td width=216 valign=top style='width:162.25pt;border:solid #BDD7EE 1.0pt;
  border-top:none;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>marlboro</p>
  </td>
  <td width=216 valign=top style='width:162.25pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>100</p>
  </td>
  <td width=216 valign=top style='width:162.3pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>0.33</p>
  </td>
 </tr>
</table>

<p class=MsoNormal style='margin-top:10.0pt;margin-right:0cm;margin-bottom:
0cm;margin-left:36.0pt;margin-bottom:.0001pt;text-indent:-36.0pt;page-break-after:
avoid;border:none'><b><span style='color:#5B9BD5'>2.3.2<span style='font:7.0pt "Times New Roman"'>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</span></span></b><b><span style='color:#5B9BD5'>Пример №2</span></b></p>

<p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt'>Входящие
параметры:</p>

<ol style='margin-top:0cm' start=1 type=1>
 <li class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;border:
     none'><span style='color:black'>Период:</span></li>
 <ol style='margin-top:0cm' start=1 type=a>
  <li class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;
      border:none'><span style='color:black'>date_from: “2019-08-01”</span></li>
  <li class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;
      border:none'><span style='color:black'>date_to: “2019-08-02”</span></li>
 </ol>
 <li class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;border:
     none'><span style='color:black'>Фильтр kkt_category: FMCG.</span></li>
 <li class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;border:
     none'><span style='color:black'>Разрезы:</span></li>
 <ol style='margin-top:0cm' start=1 type=a>
  <li class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;
      border:none'><span style='color:black'>receipt_date: true</span></li>
  <li class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;
      border:none'><span style='color:black'>region: true</span></li>
  <li class=MsoNormal style='border:none'><span style='color:black'>channel:
      true</span></li>
 </ol>
</ol>

<p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;page-break-after:
avoid'>Результат (все числа взяты случайным образом для примера):</p>

<table class=ae border=1 cellspacing=0 cellpadding=0 width=649
 style='border-collapse:collapse;border:none'>
 <tr>
  <td width=110 valign=top style='width:82.55pt;border:solid #BDD7EE 1.0pt;
  border-bottom:solid #8EAADB 1.5pt;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>receipt_date</p>
  </td>
  <td width=144 valign=top style='width:107.85pt;border-top:solid #BDD7EE 1.0pt;
  border-left:none;border-bottom:solid #8EAADB 1.5pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>region</p>
  </td>
  <td width=85 valign=top style='width:63.5pt;border-top:solid #BDD7EE 1.0pt;
  border-left:none;border-bottom:solid #8EAADB 1.5pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>channel</p>
  </td>
  <td width=97 valign=top style='width:72.65pt;border-top:solid #BDD7EE 1.0pt;
  border-left:none;border-bottom:solid #8EAADB 1.5pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>brand</p>
  </td>
  <td width=91 valign=top style='width:68.25pt;border-top:solid #BDD7EE 1.0pt;
  border-left:none;border-bottom:solid #8EAADB 1.5pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>total_sum</p>
  </td>
  <td width=123 valign=top style='width:92.0pt;border-top:solid #BDD7EE 1.0pt;
  border-left:none;border-bottom:solid #8EAADB 1.5pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>total_sum_pct</p>
  </td>
 </tr>
 <tr>
  <td width=110 valign=top style='width:82.55pt;border:solid #BDD7EE 1.0pt;
  border-top:none;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>2019-08-01</p>
  </td>
  <td width=144 valign=top style='width:107.85pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>г. Москва</p>
  </td>
  <td width=85 valign=top style='width:63.5pt;border-top:none;border-left:none;
  border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>chain</p>
  </td>
  <td width=97 valign=top style='width:72.65pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>parliament</p>
  </td>
  <td width=91 valign=top style='width:68.25pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>200</p>
  </td>
  <td width=123 valign=top style='width:92.0pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>0.67</p>
  </td>
 </tr>
 <tr>
  <td width=110 valign=top style='width:82.55pt;border:solid #BDD7EE 1.0pt;
  border-top:none;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>2019-08-01</p>
  </td>
  <td width=144 valign=top style='width:107.85pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>г. Москва</p>
  </td>
  <td width=85 valign=top style='width:63.5pt;border-top:none;border-left:none;
  border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>chain</p>
  </td>
  <td width=97 valign=top style='width:72.65pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>marlboro</p>
  </td>
  <td width=91 valign=top style='width:68.25pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>100</p>
  </td>
  <td width=123 valign=top style='width:92.0pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>0.33</p>
  </td>
 </tr>
 <tr>
  <td width=110 valign=top style='width:82.55pt;border:solid #BDD7EE 1.0pt;
  border-top:none;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>2019-08-02</p>
  </td>
  <td width=144 valign=top style='width:107.85pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>г. Москва</p>
  </td>
  <td width=85 valign=top style='width:63.5pt;border-top:none;border-left:none;
  border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>chain</p>
  </td>
  <td width=97 valign=top style='width:72.65pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>parliament</p>
  </td>
  <td width=91 valign=top style='width:68.25pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>150</p>
  </td>
  <td width=123 valign=top style='width:92.0pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>0.75</p>
  </td>
 </tr>
 <tr>
  <td width=110 valign=top style='width:82.55pt;border:solid #BDD7EE 1.0pt;
  border-top:none;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>2019-08-02</p>
  </td>
  <td width=144 valign=top style='width:107.85pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>г. Москва</p>
  </td>
  <td width=85 valign=top style='width:63.5pt;border-top:none;border-left:none;
  border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>chain</p>
  </td>
  <td width=97 valign=top style='width:72.65pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>marlboro</p>
  </td>
  <td width=91 valign=top style='width:68.25pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>50</p>
  </td>
  <td width=123 valign=top style='width:92.0pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>0.25</p>
  </td>
 </tr>
 <tr>
  <td width=110 valign=top style='width:82.55pt;border:solid #BDD7EE 1.0pt;
  border-top:none;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>2019-08-01</p>
  </td>
  <td width=144 valign=top style='width:107.85pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>г. Москва</p>
  </td>
  <td width=85 valign=top style='width:63.5pt;border-top:none;border-left:none;
  border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>nonchain</p>
  </td>
  <td width=97 valign=top style='width:72.65pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>parliament</p>
  </td>
  <td width=91 valign=top style='width:68.25pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>300</p>
  </td>
  <td width=123 valign=top style='width:92.0pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>0.75</p>
  </td>
 </tr>
 <tr>
  <td width=110 valign=top style='width:82.55pt;border:solid #BDD7EE 1.0pt;
  border-top:none;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>2019-08-01</p>
  </td>
  <td width=144 valign=top style='width:107.85pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>г. Москва</p>
  </td>
  <td width=85 valign=top style='width:63.5pt;border-top:none;border-left:none;
  border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>nonchain</p>
  </td>
  <td width=97 valign=top style='width:72.65pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>marlboro</p>
  </td>
  <td width=91 valign=top style='width:68.25pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>100</p>
  </td>
  <td width=123 valign=top style='width:92.0pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>0.25</p>
  </td>
 </tr>
 <tr>
  <td width=110 valign=top style='width:82.55pt;border:solid #BDD7EE 1.0pt;
  border-top:none;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>2019-08-02</p>
  </td>
  <td width=144 valign=top style='width:107.85pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>г. Москва</p>
  </td>
  <td width=85 valign=top style='width:63.5pt;border-top:none;border-left:none;
  border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>nonchain</p>
  </td>
  <td width=97 valign=top style='width:72.65pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>parliament</p>
  </td>
  <td width=91 valign=top style='width:68.25pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>100</p>
  </td>
  <td width=123 valign=top style='width:92.0pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>0.50</p>
  </td>
 </tr>
 <tr>
  <td width=110 valign=top style='width:82.55pt;border:solid #BDD7EE 1.0pt;
  border-top:none;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>2019-08-02</p>
  </td>
  <td width=144 valign=top style='width:107.85pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>г. Москва</p>
  </td>
  <td width=85 valign=top style='width:63.5pt;border-top:none;border-left:none;
  border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>nonchain</p>
  </td>
  <td width=97 valign=top style='width:72.65pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>marlboro</p>
  </td>
  <td width=91 valign=top style='width:68.25pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>100</p>
  </td>
  <td width=123 valign=top style='width:92.0pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>0.50</p>
  </td>
 </tr>
 <tr>
  <td width=110 valign=top style='width:82.55pt;border:solid #BDD7EE 1.0pt;
  border-top:none;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>2019-08-01</p>
  </td>
  <td width=144 valign=top style='width:107.85pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>Санкт-Петербург</p>
  </td>
  <td width=85 valign=top style='width:63.5pt;border-top:none;border-left:none;
  border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>chain</p>
  </td>
  <td width=97 valign=top style='width:72.65pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>parliament</p>
  </td>
  <td width=91 valign=top style='width:68.25pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>75</p>
  </td>
  <td width=123 valign=top style='width:92.0pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>0.33</p>
  </td>
 </tr>
 <tr>
  <td width=110 valign=top style='width:82.55pt;border:solid #BDD7EE 1.0pt;
  border-top:none;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>2019-08-01</p>
  </td>
  <td width=144 valign=top style='width:107.85pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>Санкт-Петербург</p>
  </td>
  <td width=85 valign=top style='width:63.5pt;border-top:none;border-left:none;
  border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>chain</p>
  </td>
  <td width=97 valign=top style='width:72.65pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>marlboro</p>
  </td>
  <td width=91 valign=top style='width:68.25pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>150</p>
  </td>
  <td width=123 valign=top style='width:92.0pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>0.67</p>
  </td>
 </tr>
 <tr>
  <td width=110 valign=top style='width:82.55pt;border:solid #BDD7EE 1.0pt;
  border-top:none;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>…</p>
  </td>
  <td width=144 valign=top style='width:107.85pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>…</p>
  </td>
  <td width=85 valign=top style='width:63.5pt;border-top:none;border-left:none;
  border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>…</p>
  </td>
  <td width=97 valign=top style='width:72.65pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>…</p>
  </td>
  <td width=91 valign=top style='width:68.25pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>…</p>
  </td>
  <td width=123 valign=top style='width:92.0pt;border-top:none;border-left:
  none;border-bottom:solid #BDD7EE 1.0pt;border-right:solid #BDD7EE 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal style='margin-bottom:0cm;margin-bottom:.0001pt;line-height:
  normal'>…</p>
  </td>
 </tr>
</table>

<p class=MsoNormal><b><i>Примечание</i></b>. Так как был указан фильтр, то в
отчете отображаются только данные категории FMCG.</p>

<p class=MsoNormal style='margin-left:36.0pt;border:none'>&nbsp;</p>

</div>

</body>

</html>
