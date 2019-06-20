<?php
/**
* Plugin Name: Jumlah Tulisan
* Plugin URI: http://kdesain.com
* Description: Menampilkan jumlah tulisan pada situs WordPress Multisite dengan filter per tahun dan bulan
* Version: 0.2
* Author: Ahmad Bagwi Rifai
* Author URI: https://23redstrip.wordpress.com
*/
defined( 'ABSPATH' ) or die( 'No script kiddies please!' );
function jumlah_tulisan_actions() {
     add_menu_page('Jumlah tulisan', 'Jumlah Tulisan', 'edit_pages', 'jumlah_tulisan', 'jumlah_tulisan_function', '', 86);
     add_submenu_page('jumlah_tulisan', 'Per tahun', 'Tahunan', 'edit_pages', 'per-tahun', 'per_tahun_function' );
}
add_action('admin_menu', 'jumlah_tulisan_actions');
function jumlah_tulisan_function() { ;?>
<form action="" method="POST">
    <label>Tahun</label>
    <select name="tahun">
        <option value="-"></option>
        <option value="2015">2015</option>
        <option value="2016">2016</option>
        <option value="2017" selected>2017</option>
        <option value="2018">2018</option>
        <option value="2019">2019</option>
    </select>
    <label>Bulan</label>
    <select name="bulan">
        <option value="-" selected></option>
        <option value="01">Januari</option>
        <option value="02">Februari</option>
        <option value="03">Maret</option>
        <option value="04">April</option>
        <option value="05">Mei</option>
        <option value="06">Juni</option>
        <option value="07">Juli</option>
        <option value="08">Agustus</option>
        <option value="09">September</option>
        <option value="10">Oktober</option>
        <option value="11">November</option>
        <option value="12">Desember</option>    
    </select>
    <input type="submit" name="cari" value="Cari">
</form>
<?php
    global $wpdb;
    if (isset($_POST['cari'])) {
    $tahun = $_POST['tahun'];
    $bulan = $_POST['bulan'];
    }
    $tanggal = getdate();
    if (empty($tahun) && empty($bulan)) {
        $year = $tanggal['year'];
        $month = $tanggal['mon'];
    } else {
        $year = $tahun;
        $month = $bulan;
    }
    
    
    $blogs = $wpdb->get_results( $wpdb->prepare(
            "SELECT * FROM {$wpdb->blogs} WHERE  spam = '0' 
            AND deleted = '0' AND archived = '0' 
            ORDER BY registered DESC, 2", ARRAY_A ) );

    $original_blog_id = get_current_blog_id();

    if ( empty( $blogs ) )
    {
        echo '<p>Tidak ada situs!</p>';
        return false;
    }
    ?>
   
    <h3 style="text-align: right;">Menampilkan tulisan bulan: <?php echo $month;?> tahun: <?php echo $year; ?></h3>
    <table class="widefat" data-name="post-stats">
        <thead>
            <tr>
                <th>Daftar situs</th>
                <th>Tulisan terbit</th>
                <th>Draft</th>
            </tr>
        </thead>
        <tbody>
        <?php
    
        $args = array(
        'numberposts'     => -1,
        'post_type'       => 'post',
        'post_status'     => 'publish',
        'date_query' => array(
        array(
            'year'  => $year,
            'month' => $month
        ),
        )
    );
    $total_network = $draft_network = 0;
    $total_sites = 0;

    foreach ($blogs as $blog)
    {
        wp_cache_flush();
        switch_to_blog( $blog->blog_id );
        $args['post_status'] = 'publish';
        if (count(get_posts($args))<0) { continue; }
        $total_posts = count( get_posts( $args ) );
        $total_network += $total_posts;
        $total_sites += 1;

        $args['post_status'] = 'draft';
        $draft_posts = count( get_posts( $args ) );
        $draft_network += $draft_posts;
        ?>
           <tr>
             <th><?php echo get_bloginfo( 'name' )." - "; ?><a href="<?php echo site_url(); ?>"><?php echo site_url(); ?></a></th>
             <th><?php echo $total_posts; ?></th>
             <th><?php echo $draft_posts; ?></th>
           </tr>
        <?php
    }
    ?>
           <tr>
             <td><b>Total (<?php echo $total_sites;?> situs)</b></td>
             <td><strong><?php echo $total_network; ?></strong></td>
             <td><strong><?php echo $draft_network; ?></strong></td>
           </tr>
        </tbody>
    </table>

<?php switch_to_blog( $original_blog_id );
}
//Fungsi mengambil post per tahun
function per_tahun_function() { ;?>

<form action="" method="POST">
    <label>Tahun</label>
    <select name="tahun">
        <option value="-"></option>
        <option value="2015">2015</option>
        <option value="2016">2016</option>
        <option value="2017" selected>2017</option>
        <option value="2018">2018</option>
        <option value="2019">2019</option>
    </select>

    <input type="submit" name="cari" value="Cari">
</form>

<?php
    global $wpdb;
    if (isset($_POST['cari'])) {
    $tahun = $_POST['tahun'];
}
    $tanggal = getdate();
    if (empty($tahun) && empty($bulan)) {
        $year = $tanggal['year'];
    } else {
        $year = $tahun;
    }
    
    
    $blogs = $wpdb->get_results( $wpdb->prepare(
            "SELECT * FROM {$wpdb->blogs} WHERE  spam = '0' 
            AND deleted = '0' AND archived = '0' 
            ORDER BY registered DESC, 2", ARRAY_A ) );

    $original_blog_id = get_current_blog_id();

    if ( empty( $blogs ) )
    {
        echo '<p>Tidak ada situs!</p>';
        return false;
    }
    ?>
    <h3 style="text-align: right;">Menampilkan tulisan tahun: <?php echo $year; ?></h3>
    <table class="widefat" data-name="post-stats">
        <thead>
            <tr>
                <th>Daftar situs</th>
                <th>Tulisan terbit</th>
                <th>Draft</th>
            </tr>
        </thead>
        <tbody>
        <?php
    
        $args = array(
        'numberposts'     => -1,
        'post_type'       => 'post',
        'post_status'     => 'publish',
        'date_query' => array(
        array(
            'year'  => $year,
        ),
        )
    );
    $total_network = $draft_network = 0;
    $total_sites = 0;

    foreach ($blogs as $blog)
    {
        wp_cache_flush();
        switch_to_blog( $blog->blog_id );
        $args['post_status'] = 'publish';
        if (count(get_posts($args))<0) { continue; }
        $total_posts = count( get_posts( $args ) );
        $total_network += $total_posts;
        $total_sites += 1;

        $args['post_status'] = 'draft';
        $draft_posts = count( get_posts( $args ) );
        $draft_network += $draft_posts;
        ?>
           <tr>
             <th><?php echo get_bloginfo( 'name' )." - "; ?><a href="<?php echo site_url(); ?>"><?php echo site_url(); ?></a></th>
             <th><?php echo $total_posts; ?></th>
             <th><?php echo $draft_posts; ?></th>
           </tr>
        <?php
    }
    ?>
           <tr>
             <td><b>Total (<?php echo $total_sites;?> situs)</b></td>
             <td><strong><?php echo $total_network; ?></strong></td>
             <td><strong><?php echo $draft_network; ?></strong></td>
           </tr>
        </tbody>
    </table>
<?php switch_to_blog( $original_blog_id );
}
