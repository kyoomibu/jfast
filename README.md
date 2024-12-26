# jFast Library Documentation

## AJAX Requests ($.ajax)

### Basic Syntax
```javascript
$.ajax({
    url: 'path/to/api',
    type: 'POST', // hoặc 'GET', 'PUT', 'DELETE'
    data: {
        key: 'value'
    },
    success: function(response, status, xhr) {
        // Xử lý khi thành công
    },
    error: function(xhr, status, error) {
        // Xử lý khi có lỗi
    }
});
```

### Options

| Option | Type | Mô tả |
|--------|------|--------|
| url | String | URL đích để gửi request |
| type | String | Phương thức HTTP ('GET', 'POST', 'PUT', 'DELETE'). Mặc định là 'GET' |
| data | Object/String | Dữ liệu gửi đi với request |
| dataType | String | Kiểu dữ liệu mong muốn nhận về ('json', 'text', etc.) |
| contentType | String/Boolean | Content-Type của request. Mặc định là 'application/x-www-form-urlencoded; charset=UTF-8'. Set `false` để không gửi |
| headers | Object | Headers tùy chỉnh để gửi với request |
| async | Boolean | Có thực hiện request bất đồng bộ không. Mặc định là `true` |
| beforeSend | Function | Callback được gọi trước khi request được gửi |
| success | Function | Callback khi request thành công |
| error | Function | Callback khi request thất bại |

### Ví dụ

1. GET Request đơn giản:
```javascript
$.ajax({
    url: '/api/users',
    type: 'GET',
    success: function(response) {
        console.log('Users:', response);
    }
});
```

2. POST Request với dữ liệu JSON:
```javascript
$.ajax({
    url: '/api/users',
    type: 'POST',
    contentType: 'application/json',
    data: JSON.stringify({
        name: 'John',
        age: 30
    }),
    success: function(response) {
        console.log('User created:', response);
    },
    error: function(xhr, status, error) {
        console.error('Error:', error);
    }
});
```

3. Request với headers tùy chỉnh:
```javascript
$.ajax({
    url: '/api/protected',
    headers: {
        'Authorization': 'Bearer token123',
        'Custom-Header': 'value'
    },
    success: function(response) {
        console.log(response);
    }
});
```

4. Upload file:
```javascript
var formData = new FormData();
formData.append('file', fileInput.files[0]);

$.ajax({
    url: '/upload',
    type: 'POST',
    data: formData,
    contentType: false,
    processData: false,
    success: function(response) {
        console.log('File uploaded:', response);
    }
});
```

5. Request với xử lý trước khi gửi:
```javascript
$.ajax({
    url: '/api/data',
    beforeSend: function(xhr) {
        xhr.setRequestHeader('Authorization', 'Bearer ' + getToken());
    },
    success: function(response) {
        console.log(response);
    }
});
```



## DOM Manipulation

### Selector
Thư viện hỗ trợ các selector giống như CSS:
```javascript
$('.class')         // Chọn theo class
$('#id')            // Chọn theo id
$('div')            // Chọn theo tag
$('div.class')      // Kết hợp tag và class
$('div > p')        // Chọn p là con trực tiếp của div
$('[attr=value]')   // Chọn theo attribute
```

### Class Manipulation

#### addClass(className)
Thêm một hoặc nhiều class vào elements:
```javascript
$('.item').addClass('active highlight');
```

#### removeClass(className)
Xóa một hoặc nhiều class khỏi elements:
```javascript
$('.item').removeClass('active highlight');
```

#### toggleClass(className)
Toggle một hoặc nhiều class:
```javascript
$('.item').toggleClass('active');
```

#### hasClass(className)
Kiểm tra có class hay không:
```javascript
if($('.item').hasClass('active')) {
    // Do something
}
```

### Content Manipulation

#### text(value)
Get hoặc set text content:
```javascript
// Get text
let content = $('.item').text();

// Set text
$('.item').text('New content');
```

#### html(value)
Get hoặc set HTML content:
```javascript
// Get HTML
let content = $('.item').html();

// Set HTML
$('.item').html('<span>New content</span>');
```

### Attribute Manipulation

#### attr(name, value)
Get hoặc set attribute:
```javascript
// Get attribute
let href = $('a').attr('href');

// Set attribute
$('img').attr('src', 'image.jpg');
```

#### removeAttr(name)
Xóa attribute:
```javascript
$('img').removeAttr('title');
```

### DOM Insertion

#### append(content)
Thêm content vào cuối mỗi element:
```javascript
$('.container').append('<div>New content</div>');
```

#### prepend(content)
Thêm content vào đầu mỗi element:
```javascript
$('.container').prepend('<div>New content</div>');
```

#### before(content)
Thêm content vào trước mỗi element:
```javascript
$('.target').before('<div>Before</div>');
```

#### after(content)
Thêm content vào sau mỗi element:
```javascript
$('.target').after('<div>After</div>');
```

### DOM Removal

#### remove()
Xóa elements khỏi DOM:
```javascript
$('.item').remove();
```

#### empty()
Xóa tất cả nội dung bên trong elements:
```javascript
$('.container').empty();
```

## Event Handling

### Basic Event Binding

#### on(eventName, handler)
Bind một event handler:
```javascript
$('.button').on('click', function(e) {
    console.log('Button clicked');
});
```

#### off(eventName, handler)
Unbind event handler:
```javascript
$('.button').off('click');
```

### Common Event Shortcuts

#### click(handler)
```javascript
$('.button').click(function(e) {
    console.log('Clicked');
});
```

#### change(handler)
```javascript
$('select').change(function(e) {
    console.log('Value changed');
});
```

#### focus(handler) và blur(handler)
```javascript
$('input')
    .focus(function() { console.log('Focus gained'); })
    .blur(function() { console.log('Focus lost'); });
```

### Event Delegation

```javascript
// Bind event cho elements hiện tại và tương lai
$('.container').on('click', '.button', function(e) {
    console.log('Button clicked');
});
```

### One-time Events

#### one(eventName, handler)
Bind event handler chỉ chạy một lần:
```javascript
$('.button').one('click', function(e) {
    console.log('This will only run once');
});
```


## CSS & Style Manipulation

### css(property, value)
Get hoặc set CSS properties:
```javascript
// Get một giá trị CSS
let color = $('.element').css('background-color');

// Set một giá trị CSS
$('.element').css('color', 'red');

// Set nhiều giá trị CSS
$('.element').css({
    color: 'red',
    backgroundColor: 'black',
    fontSize: '16px'
});
```

### Show/Hide/Toggle

#### show(duration)
```javascript
// Show ngay lập tức
$('.element').show();

// Show với animation
$('.element').show(300);
```

#### hide(duration)
```javascript
// Hide ngay lập tức
$('.element').hide();

// Hide với animation
$('.element').hide(300);
```

#### toggle(duration)
```javascript
// Toggle visibility
$('.element').toggle();

// Toggle với animation
$('.element').toggle(300);
```

### Fade Effects

#### fadeIn(duration)
```javascript
$('.element').fadeIn(400);
```

#### fadeOut(duration)
```javascript
$('.element').fadeOut(400);
```

### Slide Effects

#### slideUp(duration)
```javascript
$('.element').slideUp(400);
```

#### slideDown(duration)
```javascript
$('.element').slideDown(400);
```

## Form Handling

### val()
Get hoặc set form values:
```javascript
// Get value
let value = $('input').val();

// Set value
$('input').val('new value');

// Get values từ select multiple
let values = $('select[multiple]').val();
```

### serialize()
Serialize form data thành query string:
```javascript
let queryString = $('form').serialize();
// Output: "name=value&name2=value2"
```

### serializeArray()
Serialize form data thành array of objects:
```javascript
let formData = $('form').serializeArray();
// Output: [{name: "name1", value: "value1"}, {name: "name2", value: "value2"}]
```

### submit()
Submit form hoặc trigger submit event:
```javascript
// Submit form
$('form').submit();

// Handle form submission
$('form').submit(function(e) {
    e.preventDefault();
    // Handle submission
});
```

### Form Element States

#### prop()
Get hoặc set properties của form elements:
```javascript
// Check/uncheck checkbox
$('input[type="checkbox"]').prop('checked', true);

// Disable/enable input
$('input').prop('disabled', true);

// Check if checkbox is checked
if($('input[type="checkbox"]').prop('checked')) {
    // Do something
}
```

### Focus Management

#### focus()
Set focus cho element:
```javascript
$('input').focus();
```

#### blur()
Remove focus từ element:
```javascript
$('input').blur();
```

## Dimension & Position

### Dimensions

#### width() và height()
```javascript
// Get width/height
let width = $('.element').width();
let height = $('.element').height();

// Set width/height
$('.element').width(100);
$('.element').height(100);
```

### Position & Offset

#### offset()
Get vị trí element so với document:
```javascript
let position = $('.element').offset();
// Returns: { top: 100, left: 100 }
```

#### position()
Get vị trí element so với parent:
```javascript
let position = $('.element').position();
// Returns: { top: 10, left: 10 }
```

### Scroll

#### scrollTop() và scrollLeft()
```javascript
// Get scroll position
let scrollTop = $('.element').scrollTop();

// Set scroll position
$('.element').scrollTop(100);
```


## Traversing (Duyệt DOM)

### Tìm kiếm Elements

#### find(selector)
Tìm các elements con phù hợp với selector:
```javascript
$('.container').find('.item');
```

#### closest(selector)
Tìm element tổ tiên gần nhất phù hợp với selector:
```javascript
$('.item').closest('.container');
```

#### parent()
Lấy element cha trực tiếp:
```javascript
$('.item').parent();
```

#### parents()
Lấy tất cả elements tổ tiên:
```javascript
$('.item').parents();
```

#### children()
Lấy tất cả elements con trực tiếp:
```javascript
$('.container').children();
```

### Duyệt theo thứ tự

#### next()
Lấy element kế tiếp:
```javascript
$('.item').next();
```

#### prev()
Lấy element trước đó:
```javascript
$('.item').prev();
```

#### siblings()
Lấy tất cả elements cùng cấp:
```javascript
$('.item').siblings();
```

### Filtering

#### filter(selector)
Lọc elements theo selector:
```javascript
$('li').filter('.active');
```

#### not(selector)
Loại bỏ elements theo selector:
```javascript
$('li').not('.active');
```

#### first()
Lấy element đầu tiên:
```javascript
$('li').first();
```

#### last()
Lấy element cuối cùng:
```javascript
$('li').last();
```

#### eq(index)
Lấy element tại vị trí index:
```javascript
$('li').eq(2); // Lấy element thứ 3 (index bắt đầu từ 0)
```

## Utility Methods

### each()
Duyệt qua từng element và thực hiện callback:
```javascript
$('.items').each(function(index, element) {
    console.log(index, element);
});

// Static method
$.each(array, function(index, value) {
    console.log(index, value);
});
```

### map()
Chuyển đổi mảng các elements:
```javascript
// Instance method
$('.items').map(function(index, element) {
    return element.textContent;
});

// Static method
$.map(array, function(value, index) {
    return value * 2;
});
```

### clone()
Tạo bản sao của elements:
```javascript
// Clone không bao gồm event handlers
$('.element').clone();

// Clone bao gồm cả event handlers
$('.element').clone(true);
```

### index()
Lấy vị trí của element trong collection:
```javascript
// Lấy index của element trong siblings
$('.item').index();

// Lấy index của element trong selector
$('.item').index('selector');
```

### grep()
Lọc mảng với callback:
```javascript
$.grep(array, function(value, index) {
    return value > 10;
});
```

### inArray()
Kiểm tra sự tồn tại của giá trị trong mảng:
```javascript
$.inArray(value, array); // Trả về index hoặc -1
```

### Data Attributes

#### data()
Get hoặc set data attributes:
```javascript
// Get data attribute
let value = $('.element').data('key');

// Set data attribute
$('.element').data('key', 'value');
```


## Event Utilities

### trigger()
Kích hoạt event:
```javascript
$('.element').trigger('click');
```

### Shorthand Events
```javascript
// Mouse events
$('.element').click(handler);
$('.element').mouseenter(handler);
$('.element').mouseleave(handler);
$('.element').hover(enterHandler, leaveHandler);

// Keyboard events
$('.element').keydown(handler);
$('.element').keyup(handler);
$('.element').keypress(handler);

// Form events
$('.element').submit(handler);
$('.element').change(handler);
$('.element').focus(handler);
$('.element').blur(handler);
```

## Chain Methods
Tất cả các methods đều có thể chain với nhau:
```javascript
$('.element')
    .addClass('active')
    .css('color', 'red')
    .fadeIn(400)
    .text('New content')
    .click(function() {
        // Handle click
    });
