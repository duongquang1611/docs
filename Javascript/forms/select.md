## Select
### Gán giá trị cho thẻ select
```typescript
setData(ele: elementHTML, data: {value, text}[], valToSet?: any) {
	const mData = data || [];
	const val = valToSet === undefined ? $ele.val() : valToSet;

	$ele.html('');

	for (const item of mData) {
		const opt = $('<option></option>');
		opt.attr('value', item.value);
		opt.text(item.text);
		if (item.value === val) {
			opt.attr('selected', 'selected');
		}

		opt.appendTo($ele);
	}

	$e.trigger('change.select2');

	return this;
}
```
### Lấy giá trị được chọn từ thẻ select
```typescript
getValue(ele: elementHTML) {
	return $ele.val();
}
```
