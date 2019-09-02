# Seleniumメモ
## まずimport
from selenium import webdriver
これがないと話にならない
## ドライバーセットアップ
driver = webdriver.Chrome(r"C:\Users\koga\Documents\chromedriver_win32\chromedriver.exe")
これで取得したブラウザのドライバーを使って要素，中のテキストを取得，ボタンのクリック，テキストボックスへの入力を行う
## 要素取得メソッド
```サンプルHTML
<form id="search_mini_form" action="http://demo.magentocommerce.com/catalogsearch/result/" method="get">
    <div class="form-search">
        <label for="search">Search:</label>
        <input id="search" type="text" name="q" value="" class="input-text" maxlength="128" />
        <button type="submit" title="Search" class="button"><span><span>Search</span></span></button>
        <div id="search_autocomplete" class="search-autocomplete"></div>
        <script type="text/javascript">
        //<![CDATA[
            var searchForm = new Varien.searchForm('search_mini_form', 'search', 'Search entire store here...');
            searchForm.initAutocomplete('http://demo.magentocommerce.com/catalogsearch/ajax/suggest/', 'search_autocomplete');
        //]]>
        </script>
    </div>
</form>
```
- IDを探索して要素を取得
    - find_element_by_id(id)
    - `driver.find_element_by_id('search')`
        - IDをキーにして検索する．ここでは頭の`id="search`にマッチし，以下で表される要素が取得できる
        - `<input id="search" type="text" name="q" value="" class="input-text" maxlength="128" />`
- nameから取得
    - find_element_by_name(name)
    - `driver.find_element_by_name('q')`
        - nameをキーにして検索する．ここでは`name="q"`にマッチし，以下で表される要素が取得できる
        - `<input id="search" type="text" name="q" value="" class="input-text" maxlength="128" />`
- classから取得
    - find_element_by_class_name(name)
    - `driver.find_element_by_class_name('input-text')`
        - クラスをキーにして検索する．ここでは`class="input-text"`にマッチし，以下で表される要素が取得できる
        - `<input id="search" type="text" name="q" value="" class="input-text" maxlength="128" />`
- tagから取得
    - find_element_by_tag_name(name)
    - `driver.find_element_by_tag_name('input')`
        - HTMLタグをキーにして検索する．ここでは頭の`input`にマッチし，以下で表される要素が取得できる
        - `<input id="search" type="text" name="q" value="" class="input-text" maxlength="128" />`
- xpathを利用して取得
    - find_element_by_xpath(xpath)
    - `driver.find_element_by_xpath('//form[0]/div[0]/input[0]')`
        - xpathを使うこともできる．//は前を省略．`<form id="search_mini_form" hogehoge`より前に何かが記述されていても省略できる．//以下のパスが他にないことが確実であるなら//で省略すれば良い．
        - それ以降はformの0番目-divの0番目-inputの0番目．つまり，`<form id="search_mini_form" action="http://demo.magentocommerce.com/catalogsearch/result/" method="get">`の0番目の子要素の`<div class="form-search">`の0番目の子要素の`<input id="search" type="text" name="q" value="" class="input-text" maxlength="128" />`がマッチする
- CSSセレクタから取得
    - find_element_by_css_selector(css_selector)
    - `driver.find_element_by_css_selector('#search')`
        - CSSセレクタから検索することができる．.cssがあるならそれを基にこれを使うのが一番楽かもしれない
        - `<input id="search" type="text" name="q" value="" class="input-text" maxlength="128" />`
    - CSSセレクタを以下に簡単にまとめる
        - IDセレクタ: 要素名#ID名 `#search'`
        - classセレクタ: 要素名.class名 `input.input-text`
        - 要素セレクタ: 要素名
        - その他疑似要素，疑似クラス，属性セレクタ等がある．詳細は外部ページ: http://www.htmq.com/csskihon/005.shtml 参照
**複数の要素をリスト形式で取得するにはメソッド名の<span style="color: red; ">element</span>を<span style="color: red; ">elements</span>にすればよい．