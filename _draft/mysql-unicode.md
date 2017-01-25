# lấy các bài hát có chứa dấu tiếng Việt
SELECT COUNT(*)
FROM vt_song
WHERE is_active = 1 AND NAME <> CONVERT(NAME USING ASCII) ORDER BY id ASC
