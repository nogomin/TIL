### Paging
```php
<?php
class Page
{
    private $pageLinks; // 페이지 링크들을 가진 html코드.

    function __construct($page, $totalCount, $linePerPage, $blockPerScreen)
    {
        $linePerPage; //한페이지 줄 수
        $blockPerScreen; //한페이지 블럭 수

        $totalPage = ceil($totalCount / $linePerPage); //총 페이지 수
        $totalBlock = ceil($totalPage / $blockPerScreen); //총 블럭 수

        if (!$page) $page = 1;
        $block = ceil($page / $blockPerScreen); //현재 블럭

        $startRowNum = ($page -1) * $linePerPage; //글 시작 순서

        $firstPage = (($block - 1) * $blockPerScreen) + 1; // 첫번째 페이지번호
        $lastPage = min ($totalPage, $block * $blockPerScreen); // 마지막 페이지번호

        $prev = $page - 1; // 이전페이지 '<' 표시
        $next = $page + 1; // 다음페이지 '>' 표시

        // 페이지 블럭의 이동
        $prevBlock = $block - 1; // 이전블럭
        $nextBlock = $block + 1; // 다음블럭

        $prevBlockpage = $prevBlock * $blockPerScreen;
        // '<<' 눌렀을때의 페이지번호 이전 블럭에서의 마지막 페이지 번호이다.
        $nextBlockpage = $nextBlock * $blockPerScreen - ($blockPerScreen - 1);
        // '>>' 눌렀을 때 페이지 번호 다음 블럭애서의 첫 페이지 번호.

        // 처음.
        $pageLinks = "<ul class='page'><li onclick='move_page(\"1\")'><img src='images/move_first.png' alt='처음' /></li>";

        // 이전.
        if ( $prev > 0 ) {
            $pageLinks .= "<li onclick='move_page(\"{$prev}\")' ><img src='images/move_prev.png' alt='이전' /></li>";
        }
        else {
            $pageLinks .= "<li><img src='images/move_prev.png' alt='이전' /></li>";
        }

        // 페이지 번호 출력.
        for ( $i = $firstPage; $i <= $lastPage; $i++ ) {
            if ( $i > $lastPage ) break;
            if ( $i == $page ) {
                $pageLinks .= "<li class='current-page'><span>$i</span></li>";
            }
            else {
                $pageLinks .= "<li onclick='move_page(\"{$i}\")'>$i</li>";
            }
        }

        // 다음.
        if ( $next > 0 ) {
            $pageLinks .= "<li onclick='move_page(\"{$next}\")'><img src='.images/move_next.png' alt='다음' /></li>";
        }
        else {
            $pageLinks .= "<li><img src='images/move_next.png' alt='다음' /></li>";
        }
        
        // 마지막.
        $pageLinks .= "<li onclick='move_page(\"{$totalPage}\")'><img src='images/move_last.png' alt='이전' /></li></ul>";

        $this->pageLinks = $pageLinks;

    }
   
    // 객체 생성 후 이 메소드를 통해 링크코드를 가져다 html에 삽입한다.
    function getPageLinks() { return $this->pageLinks; }
}
?>
```
