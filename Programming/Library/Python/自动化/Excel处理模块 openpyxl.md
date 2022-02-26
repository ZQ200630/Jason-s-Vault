---
tags:  自动化 Python
alias: 
---
# Excel处理模块 openpyxl

```python
openpyxl.load_workbook('example.xlsx')
    get_sheet_names()  
    # 获取所有的sheet名
    get_active_sheet()  
    # 取得活动工作表，即打开的为excel打开时直接出现的
    sheet = get_sheet_by_name(name)  
    # 得到一个sheet对象
        title  
        # 此sheet名
        sheet[cellname]  
        # 返回cell
            value  
            # 返回该cell数据
            row  
            # 返回该cell行号
            column  
            # 返回该cell列号
            coordinate  
            # 返回该cell行与列
        sheet['A1':'A3']  
        # 获取此整个矩形区域的cell元素
        tuple(sheet['A1':'A3'])  
        # 在一个元组中列出cell对象
        sheet.columns[number]  
        # 返回一列
        sheet.cell(row=..., column=...)  
        # 返回cell
        sheet.row_dimensions[number].height = 70  
        # 将第一行高度设置为70
        sheet.column_dimensions['B'].width = 20  
        # 将B列宽度设置为20
        sheet.merge_cells('A1:D3')  
        # 把该矩形合并
        sheet.unmerge_cells(‘A1:d3’)  
        # 把该矩形拆分
        sheet.freeze_panes='param'  
        # param单元格左边与上边所有元素将会冻结
        sheet.freeze_panes='A1'/None  
        # 没有冻结窗口
    get_highest_column()  
    # 返回最大列
    get_highest_row()  
    # 返回最大行
    save(filename)  
    # 保存更改
    Workbook()  
    # 新建一个工作空间
        create_sheet()  
        # 新建默认表
        create_sheet(index=..., title=...)  
        # 索引与名字
        remove_sheet(wb.get_sheet_by_name(name))  
        # 删除sheet
        sheet['A1'] = '......'  
        # 将值写入单元格
        
openpyx1.cell
    get_column_letter(number)  
    # 返回列字母组合
    column_index_from_string('')  
    # 返回字符串对应的列数

from openpyx1.styles import Font, Style
    fonObj1 = Font(name='fontname', bold=True, size=24, italic=True)  
    # 生成一个fontname字体，加粗，大小为24，斜体的字体对象
    sheet['A1'].font = fonObj1  
    # 应用字体
    sheet['A3'] = 'SUM(A1:A2)'  
    # 使用公式求和(若想让sheet使用该公式，则在load_workbook中加一个参数data_only=True)
```