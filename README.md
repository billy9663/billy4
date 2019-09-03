# billy4

<!DOCTYPE HTML>
<html lang="en">
	<head>
		<title>FreeBitco.in / FreeDoge.co.in Roll Verifier</title>
		<meta http-equiv="content-type" content="text/html;charset=UTF-8">
		<link href="//fonts.googleapis.com/css?family=Open+Sans:300,400,600,700&amp;lang=en" rel="stylesheet">
		<script src="//code.jquery.com/jquery-1.11.0.min.js"></script>
		<script src="//freebitco.in/CryptoJS-master/rollups/sha256.js"></script>
		<script src="//freebitco.in/CryptoJS-master/rollups/hmac-sha512.js"></script>
		<script src="//freebitco.in/CryptoJS-master/components/enc-base64-min.js"></script>
		<script src="//freebitco.in/CryptoJS-master/rollups/sha512.js"></script>
		<style type="text/css">
			body{font-family: 'Open Sans', sans-serif;font-size:20px;text-align:center;}
			input{font-size:20px;border:2px solid gray;}
			p { margin:10px }
			h1,h2{ margin:15px }
			.bold { font-weight:bold;}
			td {text-align:center;}
		</style>
	</head>
	<body>
		<h1>FreeBitco.in / FreeDoge.co.in Roll Verifier</h1>
		
		<p>Enter the details from the section titled <b>PREVIOUS ROLL DETAILS</b> that you can see after you click on the link that says <b>PROVABLY FAIR</b> to verify that the number you rolled was indeed provably fair (ie. we did not change the roll outcome after you clicked on ROLL and that we did indeed use the same server and client seeds that we showed you before the roll, to generate the rolled number).</p>
		<p>This roll verifier is entirely coded in javascript so you can view the source to see how the roll is calculated and we will not be able to manipulate this script in any way because the calculations are done right here in your browser and the source is open for everyone to see.</p>
		<table border=0 cellpadding=5px align=center>
			<tr>
				<td class="bold">SERVER SEED : </td>
				<td class="bold">SERVER SEED HASH : </td>
			</tr>
			<tr>
				<td><input id="server_seed"></td>
				<td><input id="server_seed_hash"></td>
			</tr>
			<tr>
				<td class="bold">CLIENT SEED : </td>
				<td class="bold">NONCE : </td>
			</tr>
			<tr>
				<td><input id="client_seed"></td>
				<td><input id="nonce"></td>
			</tr>
			<tr>
				<td colspan=2><input type=submit id="verify" value="VERIFY!"></td>
			</tr>
		</table>
		<p id="verify_server_seed_hash_msg" class="bold"></p>
		<h2 id="rolled_number"></h2>
		<script type="text/javascript">
			$(document).ready(function() 
			{
				$('#server_seed').val(decodeURIComponent($.urlParam('server_seed')));
				$('#client_seed').val(decodeURIComponent($.urlParam('client_seed')));
				$('#server_seed_hash').val(decodeURIComponent($.urlParam('server_seed_hash')));
				$('#nonce').val(decodeURIComponent($.urlParam('nonce')));
				$("#verify").click(function(event) 
				{
					var server_seed = $('#server_seed').val();
					var client_seed = $('#client_seed').val();
					var nonce = $('#nonce').val();
					var server_seed_hash = CryptoJS.SHA256(server_seed).toString(CryptoJS.enc.Hex);
					var string1 = nonce.concat(":",server_seed,":",nonce);
					var string2 = nonce.concat(":",client_seed,":",nonce);
					var hmac512 = CryptoJS.HmacSHA512(string1,string2).toString(CryptoJS.enc.Hex);
					var string3 = hmac512.substring(0,8);
					var number = parseInt(string3, 16);
					var roll = (Math.round(number/429496.7295)).toFixed(0);
					$('#rolled_number').html(roll);
					if ($('#server_seed_hash').val() == server_seed_hash)
					{
						$('#verify_server_seed_hash_msg').html("<font color=green>SERVER SEED HASH MATCHES</font>");
					}
					else
					{
						$('#verify_server_seed_hash_msg').html("<font color=red>SERVER SEED HASH DOES NOT MATCH</font>");
					}
				});
			});
			$.urlParam = function(name)
			{
				var results = new RegExp('[\?&]' + name + '=([^&#]*)').exec(window.location.href);
				if (results==null){
				   return '';
				}
				else{
				   return results[1] || 0;
				}
			}
		</script>
	</body>
</html>

