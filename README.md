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
After converting the resulting table into _CSV_ format, I decided to explore its data using Pandas.
The table contained Wikipedia references in two columns, _"Original release date"_ and _"English release date_," in the format: _[number]_. I eliminated those using regular expressions:
```Python
data['Original release date'] = data['Original release date'].str.replace(r'\s*\[\d+\]\s*', '', regex=True)
data['English release date'] = data['English release date'].str.replace(r'\s*\[\d+\]\s*', '', regex=True)
```
Also, I converted the first _"No."_ column into _float_ data type:
```Python
data['No.'] = data['No.'].astype(float)
```
Finally, I converted the _"Original release date"_ column from _text_ into _datetime_ format. The dates transformed from their initial appearance, such as _"May 25, 2015"_ to the format _2015-05-25_.
```Python
data["Original release date"] = pd.to_datetime(data["Original release date"], format='%B %d, %Y')
```
