## 读取excel文件
```python
# _*_ coding:utg-8 _*_
import os
import sys
import xlrd

def read_excel():
    workbook = xlrd.open_workbook(r'F:\demo.xlsx')
    # 获取所有sheet
    print(workbook.sheet_names())  # [u'sheet1', u'sheet2']
    sheet2_name = workbook.sheet_names()[1]

    # 根据sheet索引或者名称获取sheet内容
    sheet2 = workbook.sheet_by_index(1)  # sheet索引从0开始
    sheet2 = workbook.sheet_by_name('sheet2')

    # sheet的名称，行数，列数
    print(sheet2.name, sheet2.nrows, sheet2.ncols)

    # 获取整行和整列的值（数组）
    rows = sheet2.row_values(3)  # 获取第四行内容
    cols = sheet2.col_values(2)  # 获取第三列内容
    print(rows, cols)
    
    # 获取单元格内容
    print(sheet2.cell(1, 0).value.encode('utf-8'))
    print(sheet2.cell_value(1, 0).encode('utf-8'))
    print(sheet2.row(1)[0].value.encode('utf-8'))

    # 获取单元格内容的数据类型
    print(sheet2.cell(1, 0).ctype)
```

## 导出为xlsx文件
```python
import xlwt
from io import BytesIO
import os
from datetime import datetime

from django.http import HttpResponse
import xlsxwriter


class ExportXlsx(object):
    """导出xlsx文件"""
    def __init__(self):
        self.output = BytesIO()
        self.workbook = xlsxwriter.Workbook(self.output)
        self.worksheet = self.workbook.add_worksheet()
        self.bold = self.workbook.add_format({'bold': 1})

    def set_column_width(self, column_width_dict):
        """设置列宽"""
        for key, value in column_width_dict.items():
            self.worksheet.set_column(key, key, value)

    def set_header(self, header_list):
        col = 0
        for i in header_list:
            self.worksheet.write(0, col, i, self.bold)
            col += 1

    def write_data(self, instance_list, keys, foreign_keys):
        row = 1
        print(len(instance_list), instance_list)
        for instance in instance_list:
            foreign_key_index = 0
            for index, i in enumerate(keys):
                value = getattr(instance, i)
                if isinstance(value, int):
                    self.worksheet.write_number(row, index, value)
                elif isinstance(value, datetime):
                    value = value.strftime("%Y-%m-%d %H:%M:%S")
                    self.worksheet.write_string(row, index, value)
                elif isinstance(value, str):
                    self.worksheet.write_string(row, index, value)
                else:
                    if value:
                        foreign_key = getattr(value, foreign_keys[foreign_key_index]) if foreign_keys else value.id
                        self.worksheet.write_string(row, index, str(foreign_key))
                        foreign_key_index += 1
            row += 1
        self.workbook.close()

    def export(self, header_list, instance_list, keys, file_name, foreign_keys=None):
        self.set_header(header_list)
        self.write_data(instance_list, keys, foreign_keys)
        self.output.seek(0)
        response = HttpResponse(self.output.read(),
                                content_type="application/vnd.openxmlformats-officedocument.spreadsheetml.sheet")
        response['Content-Disposition'] = "attachment; filename={}.xlsx".format(file_name)
        return response
```

## 导出为excel文件
```python
class ExportExcel(object):
    """导出excel文件"""
    def __init__(self, excel_name, sheet_name="sheet1", initial_row=0):
        # 创建一个文件对象
        self.wb = xlwt.Workbook(encoding='utf8')
        # 创建一个sheet对象
        self.sheet = self.wb.add_sheet('{}'.format(sheet_name))
        self.style_heading = xlwt.easyxf()
        self.initial_row = initial_row
        self.excel_name = excel_name

    def set_style_heading(self, strg_to_parse):
        self.style_heading = xlwt.easyxf(strg_to_parse)

    def write_heading(self, items):
        for index, i in enumerate(items):
            self.sheet.write(self.initial_row, index, i, self.style_heading)

    def write_data(self, items, keys, data_row=None):
        data_row = self.initial_row + 1 if not data_row else data_row
        for i_dict in items:
            for index, i in enumerate(keys):
                self.sheet.write(data_row, index, i_dict[i])
            data_row += 1

    def save_excel(self, path="static/excel/"):
        exist_file = os.path.exists('{path}{excel_name}.xls'.format(excel_name=self.excel_name, path=path))
        if exist_file:
            os.remove(r'{path}{excel_name}.xls'.format(excel_name=self.excel_name, path=path))
        self.wb.save('{path}{excel_name}.xls'.format(excel_name=self.excel_name, path=path))

    def export(self):
        output = BytesIO()
        self.wb.save(output)
        # 重新定位到开始
        output.seek(0)
        response = HttpResponse(output.getvalue(), content_type='application/vnd.ms-excel')
        response['Content-Disposition'] = 'attachment; filename={}.xls'.format(self.excel_name)
        response.write(output.getvalue())
        return response

```




```python
if __name__ == "__main__":
    read_excel()
```