---
layout: post
category : blog
title: "体验python的argparse"
tags : [test, selenium, python]
---

参考：  　

+ [Convert CSV to XML « Python recipes « ActiveState Code](http://code.activestate.com/recipes/577423-convert-csv-to-xml/)  
+ [Python 命令行解析工具 Argparse介绍（一） - 理想空间 - 博客园](http://www.cnblogs.com/jianboqi/archive/2013/01/10/2854726.html)  


最近要用python把csv文件转成xml 格式，后期可能会加一些其他选项，所以趁这机会就想体验一下python的命令行解析模块　argparse   
当然在python里还有其他的模块也可以实现，但这个似乎比较友好　

首先csv转xml　

        import csv 


        reader = csv.reader(open(csv_file))
        xmlfile = open(xml_file, 'w')

        xmlfile.write('<?xml version="1.0"?>' + "\n")
        # there must be only one top-level tag
        xmlfile.write('<csv_data>' + "\n")

        row_num = 0
        for row in reader:
            if row_num == 0:
                tags = row
                for i in range(len(tags)):
                    tags[i] = tags[i].replace(" ", "_")
            else:
                xmlfile.write('<row>' + '\n')
                for i in range(len(tags)):
                    xmlfile.write('    ' + '<' + tags[i] + '>' + row[i] \
                                + '</' + tags[i] + '>' + '\n'
                    )
                xmlfile.write('</row>' + '\n' )
            row_num = row_num + 1
            
        xmlfile.write('</csv_data>' + '\n')
        xmlfile.close()

加入argparse支持, 计划加两个option做尝试，　--csv  --xml 用于指定文件名（非实际使用时的选项）
    
    import argparse
    
    def csv2xml(csv_file, xml_file):
        # use function above 
        pass 
    if __name__ == "__main__":
        parser = argparse.ArgumentParser(description='covert csv to xml file')
        
        parser.add_argument('--csv', help=r'Specify csv file')
        parser.add_argument('--xml', default='xml_out.xml', help=r'Specify xml file name')
        
        args = parser.parse_args()
        
        csv2xml(args.csv, args.xml)
        
这样就好了,调用可以如下：　

        python csv2xml.py --csv securityTypes.csv
        python csv2xml.py --csv securityTypes.csv　--xml myxml_file.xml
    
    
    
