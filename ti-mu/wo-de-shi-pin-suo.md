# 12.我的文档\(缩\)

## `gridview.jsp`

在`jsp`目录下新建`gridview.jsp`

```markup
<tbody id="tab">
<c:forEach var="fb" items="${list}" varStatus="status">
	<div class="gridbox">
	<c:choose>
		<c:when test="${fn:endsWith(fb.name,'.mp4')}">
			<img src="assets/images/video.jpg" alt="no image" class="gridimg"/>
		</c:when>
		<c:when test="${fn:endsWith(fb.name,'.doc')}">
			<img src="assets/images/doc.jpg" alt="no image" class="gridimg"/>
		</c:when>
		<c:when test="${fn:endsWith(fb.name,'.docx')}">
			<img src="assets/images/doc.jpg" alt="no image" class="gridimg"/>
		</c:when>
		<c:when test="${fn:endsWith(fb.name,'.txt')}">
			<img src="assets/images/txt.jpg" alt="no image" class="gridimg"/>
		</c:when>
		<c:when test="${fn:endsWith(fb.name,'.ppt')}">
			<img src="assets/images/ppt.jpg" alt="no image" class="gridimg"/>
		</c:when>
		<c:when test="${fn:endsWith(fb.name,'.xls')}">
			<img src="assets/images/Excel.png" alt="no image" class="gridimg"/>
		</c:when>
		<c:when test="${fn:endsWith(fb.name,'.pdf')}">
			<img src="assets/images/pdf.png" alt="no image" class="gridimg"/>
		</c:when>
		<c:when test="${fn:endsWith(fb.name,'.mp3')}">
			<img src="assets/images/video.png" alt="no image" class="gridimg"/>
		</c:when>
		<c:otherwise>
			<img src="" alt="no image" class="gridimg"/>
		</c:otherwise>
	</c:choose>
		<p style="margin:0" class="p_checkbox">
			<input type="checkbox" name="checkbox" id="${status.count}" class="grid-check" onclick="show()"/>
			<span class="hidden grid-path">${fb.path}</span>
		</p>
		<span class="fbname">${fb.name}</span>
	</div>
</c:forEach>
</tbody>
```

{% hint style="info" %}
从非缩略图状态跳转到缩略图状态大致是如下的效果
{% endhint %}

![](../.gitbook/assets/gridview.png)



