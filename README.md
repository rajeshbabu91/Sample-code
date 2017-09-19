# Sample-Gulp code

'use strict';

var gulp = require('gulp');
var sass = require('gulp-sass');
let cleanCSS = require('gulp-clean-css');
var uglify = require('gulp-uglify');
var pump = require('pump');
var browserSync = require('browser-sync').create();


gulp.task('sass', () => {
  return gulp.src('./app/assets/scss/*.scss')
    .pipe(sass().on('error', sass.logError))
    .pipe(gulp.dest('./app/css'));
});

gulp.task('minify-css', () => {
  return gulp.src('./app/css/*.css')
    .pipe(cleanCSS({compatibility: 'ie8'}))
    .pipe(gulp.dest('./app/css/min'));
});

gulp.task('compress', function (cb) {
  pump([
        gulp.src('./app/assets/js/*.js'),
        uglify(),
        gulp.dest('./app/js/min')
    ],
    cb
  );
});


gulp.task('browserSync', function() {
  browserSync.init({
  	port: 3002,
    server: {
      baseDir: 'app'
    },
  });
});

gulp.task('reload', () => {
    browserSync.reload();
});

gulp.task('default', ['sass', 'minify-css', 'compress', 'browserSync', 'reload']);
