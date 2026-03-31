# 31-3-2026PMMNM
<?php
/**
 * Plugin Name: LearnPress Stats Dashboard
 */

if (!defined('ABSPATH')) exit;

function lp_get_total_courses() {
    return wp_count_posts('lp_course')->publish;
}

function lp_get_total_students() {
    global $wpdb;
    return $wpdb->get_var("SELECT COUNT(DISTINCT user_id) FROM {$wpdb->prefix}learnpress_user_items WHERE item_type='lp_course'");
}

function lp_get_completed_courses() {
    global $wpdb;
    return $wpdb->get_var("SELECT COUNT(*) FROM {$wpdb->prefix}learnpress_user_items WHERE status='completed'");
}

add_action('wp_dashboard_setup', function() {
    wp_add_dashboard_widget('lp_stats', 'LearnPress Stats', function() {
        echo "Courses: " . lp_get_total_courses() . "<br>";
        echo "Students: " . lp_get_total_students() . "<br>";
        echo "Completed: " . lp_get_completed_courses();
    });
});

add_shortcode('lp_total_stats', function() {
    return "
    <div>
        <p>Courses: " . lp_get_total_courses() . "</p>
        <p>Students: " . lp_get_total_students() . "</p>
        <p>Completed: " . lp_get_completed_courses() . "</p>
    </div>";
});
