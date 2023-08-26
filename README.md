## Web Scraping
____
This small project was oriented towards learning methods of retrieving data from _Wikipedia_. Although the first attempt went smoothly, the second attempt included challenges with ensuring column matching. The web page contained 6 tables; the first was not relevant as it contained a general description of a random series. The other five tables had the first column in the _"th"_ format, which caused a _ValueError_ due to the mismatch. The code snippet below illustrates my approach to the issue:

> To retrieve only the column names, I had to limit the list to the first 5 values.
> ```Python
> title1 = table[1].find_all('th')
> table_title1 = [title.text.strip() for title in title1]
> table_title1 = table_title1[:5]
> print(table_title1)
> ```

> Rows extraction went smoothly:
> ```Python
> col_data1 = table[1].find_all('tr')
> ```

> Considering the limitations of _table_title1_, I had to iterate through the title again to extract the values from the first column. Then, I applied the _zip_ method to iterate through the rows and the first column values. _new_row_ represented an array of values for a single row within the table:
> ```Python
> title1 = table[1].find_all('th')
> table_title1 = [title.text.strip() for title in title1]
> for row, i in zip(col_data1[1:], table_title1[5:]):
>     row_data = row.find_all('td')
>     ind_data = [data.text.strip() for data in row_data]
>     new_row = [i] + ind_data
>     df1.loc[len(df1)] = new_row
> ```
