# Composer_tips

# 17 lời khuyên để sử dụng composer 1 cách hiệu quả

Hầu hết lập trình viên PHP biết cách sử dụng Composer, thế nhưng không phải tất cả chúng ta đều sử dụng chúng 1 cách hiệu quả hoặc theo những cách tốt nhất có thể. Vì vậy tôi quyết định tổng hợp những thứ mà tôi nghĩ quan trọng trong quy trình làm việc hàng ngày của tôi.

Quản điểm chung của hầu hết các lời khuyên là "Thực hiện 1 cách an toàn", điều đó có nghĩa là nếu có nhiều cách để giải quyết 1 việc gì đó, tôi sẽ tiếp cận bằng cách ít khả năng lỗi nhất.

### Lời khuyên #1: Đọc tài liệu
Tôi nói thật đấy. Tài liệu rất tuyệt và việc bỏ ra vài giờ đồng hồ ngồi đọc nó sẽ giúp bạn tiết kiệm thời gian về lâu về dai. Bạn sẽ ngạc nhiên vì có quá nhiều thứ Composer có thể làm.

### Lời khuyên #2: Cẩn thận sự khác nhau giữa 1 "project" và 1 "library".
Điều này rất quan trọng, để biết khi nào bạn cần biết  phải tại 1 "project" hay 1 "library". Mỗi loại sẽ yêu cầu những  bài thực hành riêng biệt.

1 thư viện là 1 package có thể tái sử dụng, bạn nên thêm nó như là 1 dependency(phụ thuộc) - như là symfony/symfony, doctrine/orm hay elasticsearch/elasticsearch.

1 project thường sẽ là 1 ứng dụng,  nó phụ thuộc vào 1 vài thư viện. Nó thường không được tái sử dụng(những project khác không yêu cầu nó như là 1 dependency). Ví dụ cơ bản nhất là 1 trang web thương mại, hệ thống hỗ trợ khách hàng vân vân.

Tôi sẽ phân biệt giữa thư viện và 1 project trong các lời khuyên bên dưới

### Lời khuyên #3: Sử dụng các phiên bản phụ thuộc cụ thể cho các ứng dụng.

Nếu bạn đang tạo 1 ứng dụng, bạn nên sử dụng  các phiên bản cụ thể nhất để định nghĩa các phụ thuộc. Nếu bạn cần  phân tích các fie YAML, bạn nên đặc tả phụ thuộc như thế này "symfony/yaml": "4.0.2".

Thậm chí nếu thư viện tuân theo Semantic Versioning, có thể có  sự phá vỡ tbackwards-compatibility giữa các phiên bản nhỏ  và phiên bản vá. Ví dụ, nếu bạn đang sử dụng "symfony/symfony": "^3.1", có thể có sự khác biệt trong bản 3.2 có thể làm hỏng các test ứng dụng của bạn. Hay cũng có thể có các sửa lỗi trong PHP_CodeSniffer và nó sẽ phát hiện các vấn đề định dạng mới trong code của bạn, và 1 lần nữa nó có thể dẫn tới cấu trúc code hỏng.

Việc cập nhật các phụ thuộc cũng phải thật thận trọng,  không thể tùy tiện được. 1 trong những lời khuyên bên dưới sẽ thảo luận về điều này chi tiết hơn.

Nghe có vẻ hơi quá 1 tí, nhưng điều này sẽ ngăn việc các đồng nghiệp của bạn đột ngột cập nhật tất cả các phụ thuộc khi thêm 1 thư viện mới vào project( mà bạn có thể bỏ lỡ trong buổi Code Review).

### Lời khuyên #4: Sử dụng các khoảng phiên bản cho các phụ thuộc dạng thư viện

Nếu bạn đang tạo 1 thư viện, bạn nên định nghĩa  1 khoảng phiên bản rộng nhất có thể. Nếu bạn tạo 1 thư viện sử dụng thư viện symfony/yaml để phân tích cú pháp YAML, bạn nên require nó như thế này:

"symfony/yaml": "^3.0 || ^4.0"
Điều này có nghĩa là thư viện của bạn có thể sử dụng symfony/yaml với bất cứ phiên bản Symfony nào thuộc phiên bản 3.x hay 4.x. Điều này rất quan trọng vì  những hạn chế này sẽ được chuyển tới ứng dụng sử dụng thư viện của bạn.

Trong trường hợp có 2 thư viện với các yêu cầu  mâu thuẫn nhau, ví dụ 1 cái yêu cầu ~3.1.0 và cái kia lại yêu cầu ~3.2.0, thì quá trình cài đặt sẽ thất bại

### Lời khuyên #5: Bạn nên commit composer.lock lên git trong các ứng dụng

Nếu bạn đang tạo 1 project, bạn  chắc chắn muốn commit composer.lock lên git.  Điều này sẽ đảm bảo tất cả mọi người - cả bạn, đồng nghiệp của bạn, server CI của bạn và server product của bạn - đang chạy ứng dụng với cùng các phiên bản phụ thuộc.

Thoạt nhìn, điều này nghe có vẻ vô dụng - bạn đã  sử dụng 1 phiên bản cụ thể theo những ràng buộc được đề cập trong lời khuyên #3. Nhưng không, cũng có những phụ thuộc trong những phụ thuộc của bạn  lại không bị hạn chế bởi những ràng buộc này (ví dụ symfony/console phụ thuộc vào symfony/polyfill-mbstring). Vì vậy nếu không commit file composer.lock, bạn sẽ không  thể lấy được những tập phụ thuộc giống nhau.

### Lời khuyên #6: Để file composer.lock trong .gitignore ở các thư viện.
Nếu bạn đang tạo 1 thư viện (hãy gọi nó là acme/my-library), bạn không nên commit file composer.lock. Nó chả giúp ích được gì cho các project sử dụng thư viện của bạn đâu.

Hãy tưởng tượng rằng acme/my-library sử dụng 1 phụ thuộc monolog/monolog. Nếu bạn commit composer.lock, mọi người phát triển acme/my-library sẽ sử dụng 1 phiên bản cũ của Monolog. Nhưng khi thư viện hoàn thành, và bạn sử dụng nó trong các project thật, 1 phiên bản mới của Monolog có thể được cài đặt, và nó không còn tương thích với thư viện nữa. Nhưng bạn lại không hề đề ý đến điều đó, vì đó là composer.lock!

Vậy tốt nhất là để composer.lock vào trong .gitignore để bạn không vô tình commit nó lên.

Nếu bạn muốn chắc chắn rằng thư viện tương thích với các phiên bản khác nhau của các phụ thuộc, đọc lời khuyên tiếp theo.

### Lời khuyên 7 #7: Chạy các kiến trúc Travis CI với nhiều phiên bản phụ thuộc
Lời khuyên này chỉ áp dụng cho các thư viện mà thôi ( bởi vì bạn sử dụng các phiên bản cụ thể cho các ứng dụng)

Nếu bạn đang xây dựng 1 thư viện mã nguồn mở, bạn chắc hẳn đang sử dụng Travis CI để chạy kiến trúc của nó.

Mặc định, Composer sẽ cài đặt phiên bản  lớn nhất có thể của các phụ thuộc được ràng buộc trong composer.json cho phép. Điều này có nghĩa là ràng buộc phụ thuộc ^3.0 || ^4.0, các kiến trúc sẽ luôn sử dụng phiên bản lớn nhất của các xuất bản v4. Vì phiên bản 3.0 không bao giờ được kiểm tra, thư viện có thể sẽ không tương thích với nó và điều này có thể khiến người dùng của bạn cảm thấy không vui.

Thật may mắn là, Composer cung cấp 1 bộ chuyển đổi để cài đặt phiên bản thấp nhất có thể của phụ thuộc --prefer-lowest ( nên sử dụng --prefer-stable để tránh cài đặt các phiên bản không ổn định).

Cập nhật cấu hình .travis.yml có thể trông như thế này:
```
language: php

php:
  - 7.1
  - 7.2

env:
  matrix:
    - PREFER_LOWEST="--prefer-lowest --prefer-stable"
    - PREFER_LOWEST=""

before_script:
  - composer update $PREFER_LOWEST

script:
  - composer ci
 ```
Bạn có thể xem nó cụ thể hơn trong thư viên mhujer/fio-api-php của tôi và xây dựng  matrix trên Travis CI.

Mặc dù giải pháp này sẽ  khắc phục được hầu hết các vấn đề không tương thích, nhưng hãy nhớ rằng có nhiều sự kết hợp  trong các phụ thuộc giữa những phiên bản thấp nhất và cao nhất. Và điều này có cũng có thể trở nên không tương thích.

### Tip #8: Sắp xếp các packages khai báo trong require và require-dev theo tên
Sort packages in require and require-dev by name
It is a good practice to keep packages in require and require-dev sorted by name. It can prevent unnecessary merge conflicts when rebasing a branch. Because if you have added a package to the end of the list in two branches, there would be a merge conflict every time.
Việc sắp xếp các package trong mục require và require-dev sẽ rất có lợi. Nó có thể  tránh xung đột khi gộp không cần thiết khi tiến hành rebase 1 nhánh. Bởi vì nếu bạn thêm 1 package vào cuối danh sách trong 2 nhánh, sẽ luôn có những xung đột khi gộp.

Đây là việc buồn chán khi thực hiện bằng tay, vì vậy  đây là cách tốt nhất để cấu hình trong composer.json
{
...
    "config": {
        "sort-packages": true
    },
…
}

Lần sau, bạn require 1 package mới, nó sẽ được thêm vào  chỗ thích hợp (và không phải xuống cuối).

### Lời khuyên #9: Đừng thử gộp composer.lock khi rebase hay merge 
Nếu bạn muốn thêm 1 phụ thuộc mới vào composer.json(và composer.lock) và trước khi  nhánh của bạn được gộp, có 1 phụ thuộc khác được thêm ở master, bạn cần rebase nhánh của bạn. Và bạn sẽ gặp 1 merge-conflict trong composer.lock.

Bạn đừng nên bảo giờ thử giải quyết  xung đột này thủ công, vì file composer.lock chứa ã băm của các phụ thuộc định nghĩa trong composer.json. Vì vậy ngay cả khi bạn giải quyết được xung đột, kết quả file lock vẫn sẽ bị sai.

Điều tốt nhất có thể làm ở đây là tạo .gitattributes ở project root với dòng sau, để thể hiện rằng git sẽ không bao giờ gộp composer.lock:
/composer.lock -merge

Bạn có thể  khắc phục vấn đề này bằng các sử dụng  các nhánh tính năng ngắn hạn được gợi ý trong Trunk Based Development ( bạn nên thực hiện điều này bất cứ giá nào). Khi bạn có 1 nhánh ngắn hạn, nó sẽ được gộp kịp thời, sẽ tối thiểu rủi ro của việc  xung đột khi gộp trong composer.lock. Bạn thậm chí còn có thể tạo 1 nhánh chỉ cho việc thêm 1 phụ thuộc và gộp nó đúng cách.

Nhưng bạn phải làm gì, khi gặp xung đột khi gộp ở composer.lock khi tiến hành rebase? Giải quyết điều này với phiên bản từ master, vì vậy bạn sẽ chỉ có những thay đổi trong composer.json (những package được thêm mới). Và sau đó chạy composer update --lock, sẽ cập nhật file composer.lock với những thay đổi trong composer.json. Bây giờ bạn có thể stage file composer.lock đã được cập nhật và tiếp tục rebase.

### Lời khuyên #10: Hiểu rõ sự khác biệt giữa require và require-dev
Việc nhận thức rõ sự khác biệt giữa khối require và require-dev là thực sự quan trọng.

Các gói được require  để chạy ứng dụng hay thư viện nên được định nghĩa trong require (ví dụ như Symfony, Doctrine, Twig, Guzzle, ...) Nếu bạn đang tạo 1 thư viện, hãy cẩn trọng với những gì bạn đặt trong require. Vì mỗi phụ thuộc trong phần này cũng là 1 phụ thuộc của ứng dụng, sử dụng thư viện.

Các gói cần thiết cho việc phát triển ứng dụng(hay thư viện) nên được định nghĩa trong require-dev (ví dụ PHPUnit, PHP_codeSniffer, PHPStan).

### Lời khuyên #11: Cập nhật các phụ thuộc 1 cách an toàn

T đoán rằng chúng ta đều đồng ý 1 sự thật rằng các phụ thuộc nên được cập nhật 1 cách thường xuyên. Vấn đề tôi muốn bàn ở đây  là việc cập nhật các phụ thuộc nên được thực hiện 1 cách rõ ràng và thận trọng,  chứ không phải chỉ cần làm cho xong như các công việc khác. Nếu bạn  đang muốn cấu trúc lại 1 cái gì đó và tại cùng thời điểm với  việc cập nhật 1 vài thư viện, bạn không thể biết là ứng dụng có bị hỏng do việc cấu trúc lại hay do việc cập nhật đâu.

Bạn có thể sử dụng câu lệnh composer oudated để xem các phụ thuộc có thể được cập nhật. Bạn nến thêm lựa chọn --direct (hay -D) để chỉ hiển thị danh sách các phụ thuộc được khai báo trong composer.json. Ngoài ra còn có lựa chọn -m để chỉ hiển thị các bản cập nhật ở mức nhỏ(minor-y)

Với mỗi phụ thuộc quá hạn thì thực  hiện các bước sau đây:

- Tạo 1 nhánh mới
- Cập nhật phiên bản phụ thuộc trong composer.json lên phiên bản cuối cùng.
- Chạy composer update phpunit/phpunit --with-dependencies (thay phpunit/phpunit bằng thư viện mà bạn đang cập nhật)
- Kiểm tra CHANGELOG trong repo thư viện trên Github để xem có sự  thay đổi nào phá hỏng không. Nếu có cập nhật ứng dụng.
- Kiểm tra thử ứng dụng trên local(Nếu bạn đang sử dụng Symfony, bạn có thể thấy các cảnh báo phản đối trong Debug Bar) 
- Commit các thay đổi (composer.json, composer.lock và bất ký thứ nào khác cần thiết để phiên bản mới hoạt động.
- Đợi kiến trúc CI xong
- Merge và deploy

Đôi khi sẽ có vấn đề khi cập nhật nhiều phụ thuộc cùng lúc, ví dụ như khi bạn cập nhật Doctrine hay Symfony. Trường hợp này bạn có thể liệt kê tất cả trong câu lệnh update.

composer update symfony/symfony symfony/monolog-bundle --with-dependencies

Hoặc bạn cũng có thể sử dụng wildcard để cập nhật tất cả các phụ thuộc theo 1 namespace cụ thể:
composer update symfony/* --with-dependencies

Tôi biết rằng tất cả những điều này nghe có vẻ thừa thãi, nhưng bạn chắc chắn sẽ cập nhật các phụ thuộc 1 cách tình cờ, vậy an toàn hơn vẫn tốt mà.

1 cách ngắn ngọn mà được mọi người chấp nhận là cập nhật tất cả các phụ thuộc require-dev cùng lúc ( nếu chúng không yêu cầu những thay đổi bên trong code, mặt khác tôi khuyên bạn nên sử dụng các nhánh riêng biệt để dễ dàng review code).

### Lời khuyên #12: Bạn có thể định nghĩa các loại phụ thuộc khác trong composer.json
Ngoài việc định nghĩa các thư viện như là các phụ thuộc, bạn cũng có thể định nghĩa các thứ khác ở đây.
Bạn có thể định nghĩa phiên bản PHP nào mà ứng dụng/thư viện của bạn hỗ trợ:

"require": {
    "php": "7.1.* || 7.2.*",
},

Bạn cũng có thể định nghĩa các extensions được yêu cầu trong ứng dụng/thư viện. Điều này vô cùng hữu ích khi bạn cố gắng dockerize ứng dụng của bạn hay  khi đồng nghiệp mới của bạn  cài đặt ứng dụng lần đầu tiên.

"require": {
    "ext-mbstring": "*",
    "ext-pdo_mysql": "*",
},
(Bạn nên sử dụng * cho phiên bản extensions  vì chúng có thể không nhất quán)

### Lời khuyên #13: Xác thực composer.json trong quá trình CI build
composer.json và composer.lock nên luôn được đồng bộ. Vì thể nên có tự động kiểm tra điều này. Chỉ cần thêm đoạn sau  vào 1 phần của  đoạn mã lệnh xây dựng và nó sẽ đảm bảo composer.lock đồng bộ với composer.json)


composer validate --no-check-all --strict
### Lời khuyên #14: sử dụng Composer plugin trong PHPStorm
Có 1 composer.json plugin cho PHPStorm. Nó thêm bộ tự động hoàn thiện và 1 vài bộ xác thực khi thay đổi composer.json thủ công.

Nếu bạn đang sử dụng IDE khác(hay chỉ là các trình biên soạn code), bạn có thể  thiết lập JSON schema.

### Lời khuyên #15: Định nghĩa phiên bản production PHP  trong composer.json
Nếu bạn giống tôi và đổi khi chạy các phiên bản pre-released PHP trên local, bạn đang có gặp vấn đề trong việc cập nhật các phụ thuộc lên 1 phiên bản không hoạt động trong production. Ngy bây giờ tôi đang sử dụng PHP 7.2.0, có nghĩa là tôi có thể cài đặt các thư viện, mà không hoạt động trên 7.1. Vì production đang chạy 7.1, quá trình cài đặt sẽ thất bại.

Nhưng bạn không cần lo, có 1 cách giải quyết dễ dàng. Chỉ cần định nghĩa phiên bản production PHP trong phần config của composer.jon

"config": {
    "platform": {
        "php": "7.1"
    }
}

Đừng nhầm nó với cái trong phần require, nó khác nhau hoàn toàn. Ứng dụng của bạn có thể chạy trên cả 7.1 hay 7.2 và cùng lúc định nghĩa 7.1 như 1 nền tảng ( có nghĩa là các phụ thuộc luôn luôn cập nhật đến 1 phiên bản tương thích với 7.1)
"require": {
    "php": "7.1.* || 7.2.*"
},
"config": {
    "platform": {
        "php": "7.1"
    }
},
### Lời khuyên #16: Sử dụng các package cá nhân từ self-hosted Gitlab
Lời khuyên ở đây là sử dụng vcs như là 1 loại repository và Composer sẽ tìm ra cách đúng để lấy các package đó. Ví dụ, nếu bạn thêm 1 fork từ Github, nó  sẽ sử dụng API của nó để tải file .zip thay vì clone toàn bọ repository.

Nhưng điều này sẽ phức tạp hợp khi cài đặt cá nhân trên Gitlab. Nếu bạn sử dụng vcs như 1 loại repository, Composer sẽ phát hiện ra đây là 1 cài đặt Gitlab cố gắng tải  package bằng cách sử dụng API ( được yêu 1 API key. Tôi không muốn cài đặt nó, vì vậy tôi cài đặt thiệt lập này(sử dụng SSH để clone))

Trước tiên định nghĩa repository với loại git:
```
"repositories": [
    {
        "type": "git",
        "url": "git@gitlab.mycompany.cz:package-namespace/package-name.git"
    }
]
```
Sau đó sử dụng package mà bình thường bạn sẽ có:
```
"require": {
    "package-namespace/package-name": "1.0.0"
}
```
### Lời khuyên #17: Làm sao để tạm thời sử dụng 1 nhánh với bugfix từ fork

Nếu bạn tìm thấy 1 bug trong 1 thư viện piblic và bạn sửa nó trong fork của bạn trên Github, bạn cần phải cài thư viện từ repo này thay vì  cái chính thức( cho tới khi bugfix được gộp vào và phiên bản sửa lỗi được phát hành).

Điều này có thể được thực hiện dễ dàng bằng 1 aliasing cùng dòng:
{
    "repositories": [
        {
            "type": "vcs",
            "url": "https://github.com/you/monolog"
        }
    ],
    "require": {
        "symfony/monolog-bundle": "2.0",
        "monolog/monolog": "dev-bugfix as 1.0.x-dev"
    }
}
Bạn có thể kiểm tra bugfix trên local trước khi push nó bằng cách sử dụng đường dẫn như 1 loại repository

### Lời khuyên #18: Cà đặt prestissimo để tăng tốc độ cài đặt  package
Có 1 Composer plugin hirak/prestissimo giúp tăng tốc độ cài đặt các phụ thuộc bằng cách tải chúng theo parallel


Và điều nốt nhất ở đây là gì? Bạn chỉ cần cài đặt nó 1 lần thôi, global và nó sẽ tự động hoạt động cho tất cả các project

composer global require hirak/prestissimo
### Lời khuyên #19: Kiểm tra ràng buộc phiên bản của bạn nếu cảm thấy không chắc chắn

Viết đúng ràng buộc phiên bản có thể  là 1 việc kho khăn ngay cả sau khi đọc tài liệu.


Thật may măn, có Packagist Semver Checker để bạn có thể kiểm tra phiên bản nào phù hợp với ràng buộc được khai báo. Thay vì chỉ phân tích ràng buộc phiên bản, nó tải dữ liệu từ Packagist để hiển thị phiên bản được realease thực tế.
Xem kết quả cho symfony/symfony:^3.1.

### Lời khuyên #20: Sử dụng authoritative class map trong production
Bạn nên tạo uthoritative class map trong production. Nó làm làm tăng tốc độ tải các class bằng các include tất cả những thứ bên trong class-map và  bỏ qua bất cứ kiểm tra filesystem nào.
Bạn có thể làm điều này bằng cách chạy nó như 1 phần của quá trình xây dựng production.

composer dump-autoload --classmap-authoritative
### Lời khuyên #21: Cấu hình autoload-dev để tests
Bạn không muốn thêm các file test trong production class map(bởi vì kích thước file và bộ nhớ). Điều này có thể dễ dàng thực hiện bằng cách cấu hình autoload-dev(tương tự như autoload).
"autoload": {
    "psr-4": {
        "Acme\\": "src/"
    }
},
"autoload-dev": {
    "psr-4": {
        "Acme\\": "tests/"
    }
},
### Lời khuyên #22: Thử các mã lệnh Composer 

Các mã lệnh Composer là 1 công cụ nhẹ để tạo các lệnh cấu trúc. Tôi đã viết các chủ đề riêng về chúng.
