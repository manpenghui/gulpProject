# gulpProject
利用gulp配置的一个简单的前端脚手架

1.使用的依赖项
var gulp = require('gulp'),
    sass = require('gulp-sass'),//sass的编译
    autoprefixer = require('gulp-autoprefixer'),//自动添加css前缀
    minifycss = require('gulp-minify-css'),//压缩css
    jshint = require('gulp-jshint'),//js代码校验
    uglify = require('gulp-uglify'),//压缩js代码
      concat = require('gulp-concat'),//js代码合并
    imagemin = require('gulp-imagemin'),//压缩图片
    rename = require('gulp-rename'),
    notify = require('gulp-notify'),//更改提醒
    cache = require('gulp-cache'),//图片缓存，只有图片替换了才压缩
    livereload = require('gulp-livereload'),//自动刷新
    connect=require('gulp-connect'),//定义一个本地Server服务
    rev = require('gulp-rev'); //用于生成md5版本号
    注：MD5在项目中已被注释起来，需要的同学可以打开注释
    
    
2. 任务列表
    // Styles任务
    gulp.task('styles', function() {
        //编译sass
        return gulp.src('www/dev/sass/*.scss')
        .pipe(sass())
        //添加前缀
        .pipe(autoprefixer('last 2 version', 'safari 5', 'ie 8', 'ie 9', 'opera 12.1', 'ios 6', 'android 4'))
        //合并为all.css
        .pipe(concat('all.css'))
        //给文件添加.min后缀
        .pipe(rename({ suffix: '.min' }))
        //压缩样式文件
        .pipe(minifycss())
        //添加md5版本号
        // .pipe(rev())
        //输出压缩文件到指定目录
        .pipe(gulp.dest('www/dist/css'))
        //- 生成一个rev-manifest.json
        // .pipe(rev.manifest())
        //- 将 rev-manifest.json 保存到 rev css 目录内
        // .pipe(gulp.dest('./rev/css'))
        //提醒任务完成
        .pipe(notify({ message: 'Styles task complete' }));
    });

    // Scripts任务
    gulp.task('scripts', function() {
        //js代码校验
        return gulp.src('www/dev/js/*.js')
        .pipe(jshint())
        .pipe(jshint.reporter('default'))
        //js代码合并
        .pipe(concat('all.js'))
        //给文件添加.min后缀
        .pipe(rename({ suffix: '.min' }))
        //添加md5版本号
        // .pipe(rev())
        //压缩脚本文件
        .pipe(uglify())
        //输出压缩文件到指定目录
        .pipe(gulp.dest('www/dist/js'))
        //- 生成一个rev-manifest.json
        // .pipe(rev.manifest())
        //- 将 rev-manifest.json 保存到 rev/js 目录内
        // .pipe(gulp.dest('./rev/js'))
        //提醒任务完成
        .pipe(notify({ message: 'Scripts task complete' }));
    });
    // Images
    gulp.task('images', function() {
      return gulp.src('www/dev/img/*')
        .pipe(cache(imagemin({ optimizationLevel: 3, progressive: true, interlaced: true })))
        .pipe(gulp.dest('www/dist/img'))
        .pipe(notify({ message: 'Images task complete' }));
    });
    // Watch
    gulp.task('watch', function() {
      // Watch .scss files
      gulp.watch('www/dev/sass/*.scss', ['styles']);
      // Watch .js files
      gulp.watch('www/dev/js/*.js', ['scripts']);
      // Watch image files
      gulp.watch('www/dev/img/*', ['images']);
      // Create LiveReload server
      livereload.listen();
      // Watch any files in www/, reload on change
      gulp.watch(['www/*']).on('change', livereload.changed);
    });
    //使用connect启动一个Web服务器
    gulp.task('connect', function () {
      connect.server({
        root: 'www',
        livereload: true,
        port:12306
      });
    });
    gulp.task('default',['styles','scripts','images','watch','connect']);


3. 如何使用（确保已全局安装了gulp）
   命令行npm install或cnpm install安装依赖项，会生成一个node_modules的文件夹;
   安装完毕后，命令行gulp查看编译效果。
 
 
