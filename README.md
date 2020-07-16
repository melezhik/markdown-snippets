# markdown-snippets


```raku
for %state<packages> -> $i {
  package-install($i)
}
```

```terraform
resource "aws_instance" "example" {

  ami           = "ami-2757f631"
  instance_type = "t2.micro"
  key_name = "mylaptop"

  tags = {
    Name = "frontend"
  }
}

resource "aws_instance" "example2" {
  ami           = "ami-2757f631"
  instance_type = "t2.micro"
  key_name = "mylaptop"

  tags = {
    Name = "backend"
  }
}

resource "aws_instance" "example3" {
  ami           = "ami-2757f631"
  instance_type = "t2.micro"
  key_name = "mylaptop"

  tags = {
   Name = "database"
  }
}
```

```raku
[
  {
    host => "ec2-54-237-6-19.compute-1.amazonaws.com".Str,
    tags => "frontend,name=frontend,aws,ip=54.237.6.19,backend_ip=52.23.177.193,database_ip=54.90.19.170".Str,
  },
  {
    host => "ec2-52-23-177-193.compute-1.amazonaws.com".Str,
    tags => "backend,name=backend,aws,ip=52.23.177.193,backend_ip=52.23.177.193,database_ip=54.90.19.170".Str,
  },
  {
    host => "ec2-54-90-19-170.compute-1.amazonaws.com".Str,
    tags => "database,name=database,aws,ip=54.90.19.170,backend_ip=52.23.177.193,database_ip=54.90.19.170".Str,
  },
]
```
```raku
use JSON::Tiny;
use Data::Dump;

my $data = from-json("/home/melezhik/projects/terraform/examples/aws/terraform.tfstate".IO.slurp);
my $backend-ip;
my $database-ip;

my @aws-instances = $data<resources><>.grep({
  .<type> eq "aws_instance"
}).map({

  if .<instances>[0]<attributes><tags><Name> eq "backend" {
    $backend-ip = .<instances>[0]<attributes><public_ip>
  }
  if .<instances>[0]<attributes><tags><Name> eq "database" {
    $database-ip = .<instances>[0]<attributes><public_ip>
  }


  %(
    host => .<instances>[0]<attributes><public_dns>,
    tags => "{.<instances>[0]<attributes><tags><Name>},name={.<instances>[0]<attributes><tags><Name>},aws,ip={.<instances>[0]<attributes><public_ip>}"
  )
});

for @aws-instances -> $i {
  $i<tags> ~= ",backend_ip={$backend-ip}";
  $i<tags> ~= ",database_ip={$database-ip}"
}
```

```raku

if tags()<database> {
} elsif tags()<backend> {
} elsif tags()<fronted> {
}
```
