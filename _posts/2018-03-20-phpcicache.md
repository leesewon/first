---
layout: post
title: PHP-Ci-Cache 
---
php ci cache는 기본적으로 ci에서 제공하는 기능으로

Load
```md
$this->load->driver('cache', array('adapter' => 'apc', 'backup' => 'file'));
```

Use
```md
$cacheName = "{$paging['start']}_{$limit}_{$area1}_{$area2}_{$order}_{$query}_{$type}_{$cate}_{$deal}";
if (! $this->cache->get($cacheName)) {
    $this->cache->save($cacheName, $res, 300);
} else {
    $res = $this->cache->get($cacheName);
}
```

$this->cache->save('파일명','데이터','초');
$this->cache->get('파일명');
으로 사용할 수 있습니다.

