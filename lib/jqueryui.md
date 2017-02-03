# jQuery UI

### AutoComplete sample code
```javascript
searchTarget = $( "#site-search-input" ).autocomplete({
	create: function(){
		$(this).data('ui-autocomplete')._renderItem = function (ul, item) {
			return $('<li></li>')
			.append('<a>' + item.value + '</a>')
			.appendTo(ul);
		};
	},
	source: function( request, response ) {
		$.ajax({
			url: "demo/sample.json",
			dataType: "json",
			type: "POST",
			data: {
				q : request.term
			},
			cache: false,
			success: function(data) {
				response(data);
			}
		});
	},
	appendTo: '.search',
	minLength: 1,
	autoFocus: true,
	select: function( event, ui ) {
		location.href = "search.php?qty="+this.value;
	},
	open: function() {
		$(this).removeClass( "ui-corner-all" ).addClass( "ui-corner-top" );
	},
	close: function() {
		$( this ).removeClass( "ui-corner-top" ).addClass( "ui-corner-all" );
	}
});
```