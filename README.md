# Helpful-PHP-Snippets
Collection of some of the useful PHP Code Snippets for your next project

#### Get Server Memory Usage
```PHP
function getServerMemoryUsage(){
    $memory_usage = 'Not Available';
    $free = shell_exec('free');
    if(!nullCheck($free)){
        $free = (string)trim($free);
        $free_arr = explode("\n", $free);
        $mem = explode(" ", $free_arr[1]);
        $mem = array_filter($mem);
        $mem = array_merge($mem);
        $memory_usage = round($mem[2] / $mem[1] * 100);
    }
    return $memory_usage;
}
```
#### Get Server CPU Usage
```PHP
function getServerCpuUsage() {
    if (function_exists('sys_getloadavg')){
        $load = sys_getloadavg();
        return $load[0];
    }else {
        return 'Not Available';
    }
}
```

#### Get Domain Name
```PHP
function getDomainName($site){
    $site = clean_url($site);
    $site = parse_url('http://'.trim($site));
    $host = $site['host'];
    return $host;
}
```

#### Get User Agent
```PHP
function getUA(){
    return raino_trim($_SERVER ['HTTP_USER_AGENT']);
}
```
#### Generate Universally Unique Identifier - GUID
```PHP
function generate_uid(){
    return sprintf('%04x%04x-%04x-%04x-%04x-%04x%04x%04x',
        mt_rand(0,0xffff),mt_rand(0,0xffff),
        mt_rand(0,0xffff),
        mt_rand(0,0x0fff)|0x4000,
        mt_rand(0,0x3fff)|0x8000,
        mt_rand(0,0xffff),mt_rand(0,0xffff),mt_rand(0,0xffff)
    );
}
```

#### Function for avoiding SQL injection
```PHP
function clear($input)
{
    global $link;
    if(get_magic_quotes_gpc())
    {
        $input = stripslashes($input);
    }
    $input = strip_tags($input);
    return $input;
}}
```
#### Generate SLUG URL - SEO Friendly URL
```PHP
function slug($text){ 
  $text = preg_replace('~[^\\pL\d]+~u', '-', $text);
  $text = trim($text, '-');
  $text = iconv('utf-8', 'us-ascii//TRANSLIT', $text);
  $text = strtolower($text);
  $text = preg_replace('~[^-\w]+~', '', $text);
  if (empty($text))
  {
    return 'n-a';
  }
  return $text;
}
```
#### Create Folder
```PHP
function createFolder($path)
{       
    if (!file_exists($path)) {
        mkdir($path, 0755, TRUE);
    }
}
```
#### Generate given datetime into Time Ago String
```PHP
function timeago($datetime, $full = false) {
    $now = new DateTime;
    $ago = new DateTime($datetime);
    $diff = $now->diff($ago);

    $diff->w = floor($diff->d / 7);
    $diff->d -= $diff->w * 7;

    $string = array(
        'y' => 'year',
        'm' => 'month',
        'w' => 'week',
        'd' => 'day',
        'h' => 'hour',
        'i' => 'minute',
        's' => 'second',
    );
    foreach ($string as $k => &$v) {
        if ($diff->$k) {
            $v = $diff->$k . ' ' . $v . ($diff->$k > 1 ? 's' : '');
        } else {
            unset($string[$k]);
        }
    }

    if (!$full) $string = array_slice($string, 0, 1);
    return $string ? implode(', ', $string) . ' ago' : 'just now';
}
```
