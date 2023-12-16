//爬取https://shipin520.com/shipin-mb(部分)

package main

import (
	"bufio"
	"fmt"
	"io/ioutil"
	"net/http"
	"os"
	"regexp"
	"strings"
)

func main() {
	resp, _ := http.Get("https://shipin520.com/shipin-mb")
	all, _ := ioutil.ReadAll(resp.Body)
	//匹配
	compile := regexp.MustCompile("data-video=\"(.*?)\"")
	allString := compile.FindAllString(string(all), -1)

	//获取所有mp4得url
	urls := Spit(allString)

	getVideo(urls)

}

// Spit 处理字符 获取url
func Spit(s []string) []string {
	for i := 0; i < len(s); i++ {
		split := strings.Split(s[i], "\"")
		all := strings.ReplaceAll(split[1], "_10s", "")
		s[i] = all
	}
	//匹配字符
	return s
}

// 根据url下载视频
func getVideo(urls []string) bool {
	for i := 0; i < len(urls); i++ {

		//读取文件
		file, _ := os.OpenFile(fmt.Sprintf("mp4/%d.mp4", i), os.O_CREATE|os.O_APPEND|os.O_WRONLY, 0666)

		//发起请求 拿到数据
		http.Header{}.Set("User-Agent", "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36 Edg/120.0.0.0")
		resp, _ := http.Get("https:" + urls[i])
		content, _ := ioutil.ReadAll(resp.Body)

		writer := bufio.NewWriter(file)
		_, err := writer.Write(content)
		if err != nil {
			panic(err)
		}
		fmt.Printf("下载成功---name =%d   ---url=%s\n", i, urls[i])
	}
	return true
}
